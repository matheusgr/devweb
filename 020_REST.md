No final do roteiro seu projeto deve ter:
* Uma REST bem definida, retornando entidades JSON mesmo que de maneira artificial
* Uma biblioteca de log de requisições (ex.: morgan)
* Arquivos estáticos sendo carregados
* Testes (supertest, opcional: mocha / chai / tape)
* Documentação (swagger, opcional, swagger-ui)

# REST

A arquitetura REST têm como objetivo definir a comunicação entre servidor e cliente. Existem características importantes do REST.

* Na arquitetura REST é preciso definir RECURSOS. Cada RECURSO representa um conceito do seu sistema e que pode ser recuperado, editado, criado ou apagado.
  * O recurso pode ser um conceito como aluno, carrinho de compra, turma, …
  * Um recurso REST não é o mesmo que uma entidade no teu banco de dados. Você pode ter o recurso ‘usuário’ e ‘aluno’ e eles apontarem para a mesma entidade no banco de dados… mas representarem conceitos distintos na REST
  * Cada recurso é identificado por um identificador universal (URI)
    * /aluno → representa todos os alunos
    * /aluno/1141003 → representa um aluno
    * /turma/1/aluno/1 → representa o primeiro aluno da turma
    * /usuario/me → representa o perfil do usuário no sistema
  * Recursos podem ter sub-recursos. Uma turma ser composta por alunos.
    * As operações que você pode fazer nos recursos são determinados pelas operações HTTP (GET, POST, PUT, DELETE)

# Express

Agora você deve modelar a API REST do seu projeto. Cada endpoint (ponto de acesso do recurso) do sistema deve também receber e retornar algo além de texto, como era feito no roteiro 1. Para isso você deve configurar o tipo de resposta no Express.

Abaixo podemos ver uma opção que configura o HEADER da resposta (res). O HEADER é um conjunto de informações de cabeçalho enviado e recebido em uma requisição HTTP. Neste caso, estamos dizendo que essa resposta vai retornar um JSON. JSON é um formato de representação de entidades. Em seguida, uma String com a representação JSON de um dicionário é enviado como resposta.

```javascript
app.get('/', function(req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ a: 1 })
});
Express têm o conceito de middlewares. O middleware é uma entidade que intercepta uma chamada HTTP e interfere fazendo alguma ação. Como exemplo, é possível definir automaticamente o cabeçalho de todas as respostas, como mostrado abaixo (leia mais aqui):
app.use(function (req, res, next) {
    res.header('Content-Type', 'application/json');
    next();  // sem o next, a chamada para aqui
});

app.get('/api/endpoint1', (req, res) => {
    res.send(JSON.stringify({value: 1}));
});
```


* Você pode usar também o res.json(entidade) para gerar uma resposta que automaticamente define o cabeçalho e cria a string json.

## Body-Parser

Como estamos utilizando express 4.x, para obter o corpo (body) das requisições (geralmente POST, PUT ou PATCH) em req (req.body), precisamos utilizar uma biblioteca chamada [body-parser](https://github.com/expressjs/body-parser), para tal:
* Instale a biblioteca através do NPM (ou seja uma pessoa mais feliz usando [yarn](https://github.com/yarnpkg/yarn)) com o comando `npm install --save body-parser`

Aqui você provavelmente já conhece o conceito de middlewares , o que vamos fazer aqui adicionar mais um, a nível de aplicação. O arquivo que você importa o express vai ficar com essa cara:

```javascript
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

// faz o parse de requisições com o corpo do tipo application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }));

// faz o parse de requisições com o corpo do tipo application/json
app.use(bodyParser.json());

app.use(function (req, res, next) {
    res.header('Content-Type', 'application/json');
    next();  // sem o next, a chamada para aqui
});

app.post('/', function (req, res) {
  // aqui estamos devolvendo ao cliente o corpo da requisição POST realizada pelo mesmo.
  res.end(JSON.stringify(req.body, null, 2))
});
```

Se você estiver querendo saber sobre o porquê de termos que usar body-parser, dê uma olhada [aqui](https://medium.com/@adamzerner/how-bodyparser-works-247897a93b90).

# É hora de melhorar sua aplicação. :)

Primeiro, é hora de fazer uso do que o nodejs/express têm a oferecer. Vamos definir um conjunto de bibliotecas no nosso projeto.

## Morgan

Acrescente o MORGAN para interceptar as chamadas, você pode usar outros mecanismos de log no seu sistema.
* Instale o morgan através do NPM. Com a chamada `npm install --save morgan`, não só o morgan é instalado, como ele é automaticamente adicionado ao package.json. 
* Veja informações de como usar no [site do morgan](https://github.com/expressjs/morgan)

## Static-Files

É preciso agora servir alguns arquivos de forma estática (ou seja, arquivos que estão em diso para o usuário) utilizando o conceito de [static-files](https://expressjs.com/en/starter/static-files.html).
* Crie um diretório /static no seu projeto e compartilhe esses arquivos através de seu servidor HTTP
* Sinta-se à vontade para acrescentar mais bibliotecas interessantes ao seu projeto.

## Testes

Sim, **VOCÊ VAI PRECISAR FAZER TESTES**.

Instale o supertest ( https://www.npmjs.com/package/supertest ) como uma dependência do nodejs. Ao contrário do morgan e outras bibliotecas, instale e defina que essa é uma dependência usada somente para testes com o comando `npm install supertest --save-dev`.

Coloque um pequeno teste dentro da sua aplicação (não precisa testar todos os endpoints por enquanto).

```javascript
const request = require('supertest');
const express = require('express');
 
const app = express();
 
app.get('/user', function(req, res) {
  res.status(200).json({ name: 'john' });
});
 
request(app)
  .get('/user')
  .expect('Content-Type', /json/)
  .expect('Content-Length', '15')
  .expect(200)
  .end(function(err, res) {
    if (err) throw err;
  });
```

### Organizando seus testes.

* Faça com que o seu app seja acessível pelo teste, para isso acrescente a linha `module.exports = app` no código da sua aplicação (provavelmente o arquivo de nome index.js, app.js ou server.js). Dessa forma, quem importar este módulo, terá acesso ao app.
* Crie um diretório de testes (test) no seu projeto
* Crie um arquivo de teste (app.test.js) com o seguinte conteúdo:

```javascript
const app = require(../index); // ou server... ou app.
const request = require('supertest');

request(app)
  .get('/user')
  .expect('Content-Type', /json/)
  .expect('Content-Length', '15')
  .expect(200)
  .end(function(err, res) {
    if (err) throw err;
  });
```

* Bônus:
  * Organize seus testes usando tape ou mocha/chai. :)

## JSDoc

É preciso documentar o que você está fazendo

* Instale o [swagger](https://www.npmjs.com/package/swagger-express) e construa JSDocs para a sua aplicação.
* Você pode oferecer a documentação do swagger em uma UI através do [swagger-ui](https://www.npmjs.com/package/swagger-ui-express)

## Módulos

Organize seus módulos.

No lugar de toda aplicação existir em um único arquivo, quebre seu sistema em diferentes funcionalidades.

Crie um arquivo com parte de uma funcionalidade/macro recurso da sua aplicação. Por exemplo, professor.js e coloque o código abaixo:

```javascript
var express = require('express')
var router = express.Router()

// middleware that is specific to this router
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})
// define the home page route
router.get('/', function (req, res) {
  res.send('Profs')
})
// define the about route
router.get('/1', function (req, res) {
  res.send('Prof')
})

module.exports = router
```

Na sua aplicação, importe esse módulo e coloque ele como um middleware de sua app

```javascript
var profs = require('./professor')
// ...
app.use('/profs', profs)
```

A organização da sua aplicação não se limita apenas a criação de rotas. Se a lógica de um código estiver pesada, é comum a criação de um módulo auxiliar com a lógica de negócio. Por exemplo, faria sentido criar um professor.service.js com funções que calculam o salário de um professor de acordo com seu número de horas, define a pontuação para progressão, etc.
