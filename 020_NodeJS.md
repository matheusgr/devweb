# NodeJS

Procure se familiarizar com o nodejs ([guia](https://nodejs.org/en/docs/guides/)).

## Programa de Leitura de dados

Faça um programa para ler arquivos e processar dados de um arquivo de forma síncrona (bloqueando enquanto a leitura é feita). Veja o exemplo.

```javascript
const fs = require('fs');
const data = fs.readFileSync('/file.md'); // blocks here until file is read
// Você pode fazer isso de forma assíncrona (quando a leitura for completa uma função será chamada
const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
});
```

1. Existem alguns conceitos explorados aqui que são do interesse
   1. require: para importar módulos
   1. => (arrow functions): definem funções anônimas (funções que não precisam de uma definição de nome:
   1. const: criação de referências usadas apenas para leitura

## HTTP

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
1. Entre no site [http://localhost:3000](http://localhost:3000)
1. Olhe os pacotes na aba de rede

## Express

Prepare a instalação do express ([guia de instalação](https://expressjs.com/en/starter/installing.html)):

1. Crie um diretório ou use o diretório do projeto
1. Execute o `npm init` para iniciar o controle de pacotes pelo npm
   1. Você verá perguntas genéricas para preparar o projeto.
1. Agora faça a instalação do npm com o comando `npm install express --save`
1. O express é um framework para permitir a construção de aplicações web ([https://expressjs.com/en/starter/hello-world.html](https://expressjs.com/en/starter/hello-world.html)) teste a aplicação abaixo que faz uso do express.

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

## Operações HTTP

Existem diferentes operações que podem ser feitas em um servidor web, 4 operações básicas são descritas como:

OPERAÇÂO | SIGNIFICADO
----------------------
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
