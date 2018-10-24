# Dicas

Para as pessoas que usam VSCode, essas facilidades podem ser utilizadas

## Extensões
* Bracket Pair Colorizer
* ES7 React/Redux/GraphQL/React-Native snippets
* Live Server (Cria um servidor local para ver páginas HTML)
* Node.js Modules Intellisense

## Configurações
* Arquivo > Preferências > Configurações (Ou CTRL + ,) e cole essas configurações lá.
```javascript
{
    "editor.fontSize": 14,
    "editor.tabSize":2,
    "editor.wordWrap": "on",
    "terminal.integrated.fontSize": 14,
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    },
    "emmet.syntaxProfiles": {
        "javascript": "jsx"
    },
    "editor.formatOnSave": true
}
```

# Projeto

## Características

* Deve seguir algumas regras para o seu bem-estar:
* Deve te deixar feliz ao fazer!
* Deve ser divertido fazer!
* Deve te dar orgulho de fazer!
* Deve ser algo que você vai aguentar fazer por 1 período inteiro!

## Obrigações

* público
* individual (dois alunos podem ter o mesmo tema, mas cada um deve tentar dar a ‘sua cara’/personalização)
* algo que vc possa terminar (não precisa ser completo, mas deve ser algo utilizável no final da disciplina)
* algo que você nunca fez antes

## Sobre Requisitos e Tecnologia

* Deve ter uma lógica que envolva mais de 1 tipo de dado (ex.: questões, códigos, alunos, turmas)
* Os dados devem ter algum tipo de relação (ex.: o código é referente a uma questão e aluno)
* Deve ter mais de uma tela (ex.: tela inicial, listagem de questões)
* Deve exercitar diferentes ações nas entidades (ex.: cadastrar turmas, apagar turmas, listar alunos, editar questões)
* Deve ter mais de um tipo diferente de usuário (ex.: administradores e usuários comuns)
* Deve, obrigatoriamente, usar as tecnologias NodeJS/Express e React
  * Mas…. eu gosto de Vue → eu também, daí vc faz tudo em React e depois faz também em Vue
    * Mas... eu gosto de angular → eu não, é a vida…
      * Mas… eu gosto de angularjs → (┛◉Д◉)┛彡┻━┻

## Sugestões
* Loja Virtual
* Venda de serviços
* Venda de produtos
* Troca de produtos/serviços
* Pode arriscar algo inovador… arrisque-se
* Fórum de mensagens com algum foco específico
* Sistema de apoio educacional
* Exercícios online (Dirlididi - http://dirlididi.com)
* Flashcards/Resumos/Mapa mentais
* Um “Tinder” para conceitos que você sabe/não sabe… e te casa com pessoas com conhecimento ‘oposto’ ao seu
* Acompanhamento de Aulas
* Sistema de apoio à democracia
* Avaliação de disciplinas 😶
* Votação de pautas do Caesi
* Sugestão de ações para os guardians

# Organização de Código

1. Crie DOIS repositórios no github/lab/bitbucket para o seu projeto (só precisa fazer essa criação quando definir o que irá fazer, até lá pode ir para o passo 3)
   1. Você pode usar qualquer canto que hospede projetos GIT, mas o repositório deve permitir o cadastro de issues por todos os usuários
   1. Um repositório será para o backend (lógica) da aplicação
   1. Outro repositório será usado para o frontend (interface) da aplicação
   1. Público
   1. Linguagem: Javascript
   1. Preencha o [formulário inicial de cadastro](https://goo.gl/forms/GDfpofr1lpqr73uZ2)

# Tecnologias

1. Neste momento usaremos 3 tecnologias:
   1. NodeJS: O interpretador javascript que utilizaremos na primeira parte da disciplina.
   1. Express: Framework para definição de rotas e serviços HTTP (protocolo de acesso web)
   1. NPM: Gerente de pacotes javascript para o nodejs
1. Caso você se sinta confortável com node/express/npm pode pular as próximas partes e começar a preparar uma API REST para o seu projeto. :)

## NodeJS

Procure se familiarizar com o nodejs ([guia](https://nodejs.org/en/docs/guides/)).

### Programa de Leitura de dados

Faça um programa para ler arquivos e processar dados de um arquivo de forma síncrona (bloqueando enquanto a leitura é feita). Veja o exemplo.

```javascript
const fs = require('fs');
const data = fs.readFileSync('/file.md'); // blocks here until file is read
Você pode fazer isso de forma assíncrona (quando a leitura for completa uma função será chamada
const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
});
```

1. Existem alguns conceitos explorados aqui que são do interesse
   1. require: para importar módulos
   1. => (arrow functions): definem funções anônimas (funções que não precisam de uma definição de nome:
   1. const: criação de referências usadas apenas para leitura

### HTTP

Você pode usar o nodejs para criar um pequeno servidor web na porta 3000, como indicado no programa abaixo.

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

Para usar o servidor, siga os passos indicados:
1. Acesse esse servidor e aproveite para olhar as requisições no seu browser
1. No firefox, aperte Ctrl + Shift + E e no Chrome aperte ress  Ctrl + Shift + C e selecione a aba de rede (Network) para ver as requisições trafegando
1. Entre no site http://localhost:3000
1. Olhe os pacotes na aba de rede

## Express

Prepare a instalação do express ([guia de instalação](https://expressjs.com/en/starter/installing.html)):

1. Crie um diretório ou use o diretório do projeto
1. Execute o `npm init` para iniciar o controle de pacotes pelo npm
   1. Você verá perguntas genéricas para preparar o projeto.
1. Agora faça a instalação do npm com o comando `npm install express --save`
1. O express é um framework para permitir a construção de aplicações web (https://expressjs.com/en/starter/hello-world.html ) teste a aplicação abaixo que faz uso do express.

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

### Operações HTTP

Existem diferentes operações que podem ser feitas em um servidor web, 4 operações básicas são descritas como:
  
OPERAÇÂO | SIGNIFICADO
-----------------------
GET | Para oferecer dados
POST | Para inserir/criar dados
PUT | Para alterar dados
DELETE | Para apagar dados

O express permite que você defina o que acontece para cada uma dessas operações, como nos exemplos abaixo.

```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
})
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

Experimente realizar as chamadas acima a partir do seu navegador (alterando a chamada e a enviando)

Pronto! Esta será a base de construção do seu sistema. Essas operações serão a base para controlar os dados do seu sistema.

