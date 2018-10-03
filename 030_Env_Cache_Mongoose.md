No final do roteiro seu projeto deve ter:
* O uso de uma variável de ambiente
* Uma biblioteca de cache
* Entidades mapeadas em banco de dados


# Variáveis de Ambiente

O seu programa NodeJS pode executar em produção ou na sua máquina durante o desenvolvimento. Mesmo quando o sistema está em produção, ele pode ser executado com diferentes bancos de dados ou outros subsistemas. Neste sentido, é costume controlar a execução através de varíaveis do ambiente.

No NodeJS o controle baseando-se em variáveis de ambiente pode ocorrer através do `process.env`. Veja um exemplo de execução com uma variável de ambiente:

```javascript
PORT=9999 node app.js
```

E um exemplo de uso ([fonte](https://www.twilio.com/blog/2017/08/working-with-environment-variables-in-node-js.html)):

```javascript
const app = require('http').createServer((req, res) => res.send('Ahoy!'));
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
```

Use uma variável de ambiente para detectar se o sistema está executando em produção, ou não. Caso não esteja, imprima uma mensagem através do `console.log`.

# Memory-cache

Algumas operações realizadas no seu sistema podem ser custosas. Neste sentido, é comum fazer uso de uma memória auxiliar rápida para armazenar informação temporária.

Essa memória auxiliar é chamada de *cache*. No NodeJS, fazemos uso do [memory-cache](https://www.npmjs.com/package/memory-cache) como uma biblioteca de controle de cache rápido.

Para isso, primeiro,

1. Instale o memory-cache com o comando `npm install memory-cache --save`
1. Inicie uma instância do memory-cache (`var cache = require('memory-cache');`);
1. Armazene dados no cache (`cache.put('foo', 'bar');`)
1. Recupere dados do cache (`cache.get('foo')`)

Veja o exemplo de aplicação abaixo com o uso do cache.

```javascript
var cache = require('memory-cache');
 
// now just use the cache
 
cache.put('foo', 'bar');
console.log(cache.get('foo'));
 
// that wasn't too interesting, here's the good part
 
cache.put('houdini', 'disappear', 100, function(key, value) {
    console.log(key + ' did ' + value);
}); // Time in ms
 
console.log('Houdini will now ' + cache.get('houdini'));
 
setTimeout(function() {
    console.log('Houdini is ' + cache.get('houdini'));
}, 200);
 
 
// create new cache instance
var newCache = new cache.Cache();
 
newCache.put('foo', 'newbaz');
 
setTimeout(function() {
  console.log('foo in old cache is ' + cache.get('foo'));
  console.log('foo in new cache is ' + newCache.get('foo'));
}, 200);
```

# Mongoose e Sequelize

Por fim, precisamos conseguir armazenar dados. Costumeiramente os dados são armazenados em um sistema de banco de dados que segue uma das duas estratégias abaixo:
* **Relacional** BDs com uma estrutura que foca na relação das entidades de maneira fixa e seguindo regras bem definidas impostas pelo próprio banco
* **Não-relacional** BDs com uma estrutura que não foca na relação direta entre os elementos. São exemplos:
  * BDs de dicionário: Armazenam chave-valor
  * BDs de documentos: armazenam documentos que, internamente, podem fazer referências a outros elementos (mas a estrutura em si não é armazenada, só a referência)
  * BDs de grafos: armazenam documentos que, explicitamente, podem referência outros elementos por uma identificação.

Em NodeJS existem duas tecnologias para mapear objetos em banco de dados, o [Sequelize](http://docs.sequelizejs.com/) (ORM - Mapeamento Objeto Relacional) e o [Moongose](https://mongoosejs.com/docs/) (ODM - Mapeamento Objeto Documento).

## Usando o Mongoose

Neste exemplo, vamos discutir alguns elementos do Mongoose que também são válidos no Sequelize.

Para fazer uso do mongoose:
1. instale o Mongoose (`npm install mongoose`).
1. crie uma conexão com o seu banco de dados:

```javascript
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
```
Caso precise fazer uso de uma instalação local do mongo para testes e controle, você pode fazer uso da biblioteca `mongodb-memory-server`. Veja exemplo de uso abaixo:

```javascript
import mongoose from 'mongoose';
import MongodbMemoryServer from 'mongodb-memory-server';

const mongoServer = new MongodbMemoryServer();

mongoose.Promise = Promise;
mongoServer.getConnectionString().then((mongoUri) => {
  const mongooseOpts = { // options for mongoose 4.11.3 and above
    autoReconnect: true,
    reconnectTries: Number.MAX_VALUE,
    reconnectInterval: 1000,
    useMongoClient: true, // remove this line if you use mongoose 5 and above
  };

  mongoose.connect(mongoUri, mongooseOpts);
```

Depois de estar conectado ao Mongoose, é preciso definir o esquema dos dados com o qual você irá operar. O esquema define uma estrutura de objetos que determina campos e o modo de mapeamento do objeto para a base de dados. Veja o exemplo abaixo de um esquema que determina um elemento com um nome, e que gera um objeto que tem um método (`speak`):

```javascript
var kittySchema = new mongoose.Schema({
  name: String
});
kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}
var Kitten = mongoose.model('Kitten', kittySchema);

var fluffy = new Kitten({ name: 'fluffy' });
fluffy.speak(); // "Meow name is fluffy"
```

## Salvando objetos

Para salvar, basta realizar o `save` do objeto. Você pode ser informado de qualquer erro através de um callback.

```javascript
fluffy.save(function (err, fluffy) {
  if (err) return console.error(err);
  fluffy.speak();
});
```

## Recuperando dados

Para retornar um objeto, você pode tanto usar a semântica de consulta geral (todos os elementos) como uma busca específica se baseando em uma Regex.

```javascript
Kitten.find(function (err, kittens) {
  if (err) return console.error(err);
  console.log(kittens);
})

Kitten.find({ name: /^fluff/ }, callback);
```

Construa o modelo de dados para o seu sistema usando um mapeamento ORM, ODM ou OGM
