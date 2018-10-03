No final do roteiro seu projeto deve ter:
* Configuração CORS em Produção
* Passport (com Session)
* Passport (JWT)

# CORS

Um problema de segurança comum é o da personificação, ou seja, de um site Malicioso se passar por site Alvo.

Uma das maneiras de se fazer isto é desenvolver um frontend controlado por Malicioso que faz uso dos dados do site Alvo de forma que o usuário tenha a impressão de que está se comunicando com o Alvo.

Por exemplo, se existe uma rota de login onde o email e senha é passado o site malicioso poderia usar essa rota para fazer a autenticação e interagir com o usuário capturando a senha do mesmo.

Existem outros tipos de ataque em que, mesmo estando no site Alvo, uma injeção de código poderia fazer com que uma chamada dessa natureza fosse interceptada por um site Malicioso.

Um mecanismo simples para evitar essa personificação é limitar que domínios podem fazer uso do seu backend. Para isso configuramos o CORS (Cross-Origin Resource Sharing -- compartilhamento de recursos para diferentes origens).

A configuração do CORS é direta através do módulo [cors](https://www.npmjs.com/package/cors). Você deve instalar o pacote para utilizá-lo na sua aplicação.

## Exemplo de Uso

O exemplo abaixo mostra o uso do CORS interceptando todas as chamadas e permitindo o acesso de qualquer origem.

```javascript
var express = require('express')
var cors = require('cors')
var app = express()

app.use(cors())

app.get('/products/:id', function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for all origins!'})
})
```

Caso queira, você pode configurar o `cors` apenas para rotas específicas.

```javascript
app.get('/products/:id', cors(), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for a Single Route'})
})
```

Ou, você pode determinar explicitamente as rotas de origem, bem como configurações de uso, como mostra o exemplo abaixo.

```javascript
var corsOptions = {
  origin: 'http://example.com',
  optionsSuccessStatus: 200 // some legacy browsers (IE11, various SmartTVs) choke on 204
}

app.get('/products/:id', cors(corsOptions), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for only example.com.'})
})
```

Libere o CORS para sua aplicação quando ela NÃO estiver em ambiente de produção. Quando estiver em ambiente de produção, libere apenas para um domínio próprio.

# Autenticação e Autorização

Costumeiramente uma aplicação deve operar dentro do contexto de cada usuário. Para isto é importante realizar a `autenticação` do usuário, ou seja, identificar se o usuário é quem ele diz que é. Depois de autenticado, o usuário deve acessar apenas as rotas que ele está `autorizado`.  A autorização dita que o usuário só pode acessar aquilo que é permitido.

O mecanismo mais comum de autenticação ainda é email e senha. Seja cadastrado diretamente no sistema ou seja através de um serviço de terceiro (como google, facebook, twitter). Hoje em dia já existe autenticação por aplicação própria de celular, por link de email ou por SMS/ligação.

Nesta aula vamos explorar o uso de autenticação por login/senha e as diferentes maneiras de operar com essa autenticação.

# Passport (Session)

Nós usaremos o [passport](http://www.passportjs.org/docs/) como mecanismo de autenticação e autorização.

Para preparar o passport é preciso iniciar o express com suporte a sessões e inicializar o passport, como exibido abaixo.

```javascript
const express = require('express')
const passport = require('passport')
const session = require('express-session')

const app = express()
app.use(session({ secret: 'passport-tutorial', cookie: { maxAge: 60000 }, resave: false, saveUninitialized: false }));
app.use(passport.initialize())
app.use(passport.session())
```

O passport fará uso do conceito de SESSÃO. Uma sessão representa uma série de interações entre um usuário e o seu servidor. É identificado por um código que o usuário envia a cada requisição.

O sistema de autenticação passport fará uso de sessão para associar um usuário a uma sessão depois que o mesmo for autenticado.

Para o passport definir de onde ele consultará os usuários para fazer o login, é preciso usar uma estratégia. Usaremos a estratégia local para autenticar um usuário, como descrita a seguir:

```javascript
passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) { return done(null, false); }
      if (!user.verifyPassword(password)) { return done(null, false); }
      return done(null, user);
    });
  }
));
```
Depois que a estratégia de autenticação está definida, você pode construir um endpoint de login que será autenticado de acordo com a estratégia local definida anteriormente.

```javascript
app.post('/login', (req, res, next) => {
  passport.authenticate('local', (err, user, info) => {
    req.login(user, (err) => {
      return res.send('You were authenticated & logged in!\n');
    })
  })(req, res, next);
})
```

Por fim, você pode consultar se uma rota está autenticada através do método isAuthenticated() que é disponibilizado em todas as requisições pelo passport.

```javascript
app.get('/authrequired', (req, res) => {
  if(req.isAuthenticated()) {
    res.send('you hit the authentication endpoint\n')
  } else {
    res.redirect('/')
  }
})
```

(Mais informações [aqui](https://medium.com/@evangow/server-authentication-basics-express-sessions-passport-and-curl-359b7456003d))

# Passport Bearer
https://github.com/jaredhanson/passport-http-bearer

# Passport JWT

TODO

https://blog.gds-gov.tech/our-considerations-on-token-design-session-management-c2fa96198e6d

https://www.npmjs.com/package/passport-jwt

https://medium.com/front-end-hacking/learn-using-jwt-with-passport-authentication-9761539c4314
