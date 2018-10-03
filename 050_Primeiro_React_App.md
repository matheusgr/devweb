Vamos começar instalando o create-react-app

`npm install create-react-app`

E execute o create-react-app resultante da instalação:

`node node_modules/create-react-app/createReactApp.js nome_do_seu_projeto`

Vamos entender a estrutura do projeto criado.

A estrutura do `package.json` é um bom indicativo do que há no projeto. Temos 3 dependências:

* `react` framework de desenvolvimento web com a definição de componentes
* `react-dom` biblioteca de renderização e manipulação do DOM (document object model) para o render
* `react-scripts` scripts utilitários para a execução do react

E 3 scripts principais:

* `npm start` inicia um servidor de desenvolvimento que disponibiliza os arquivos js para o seu acesso
* `npm run build` prepara o código para produção
* `npm test` executa os testes do distema

# src

Vamos agora olhar a estrutura gerada no `src`:

* `serviceWorker.js` Um [service worker](https://developers.google.com/web/fundamentals/primers/service-workers/) é um script executado em background pelo seu browser e que permite adicionar algumas funcionalidades como controle de notificações, cache e sincronização. O react prepara um service worker para uso de cache.
* `App.test.js` Teste do App.js
* `index.js` Página inicial react. Usada apenas para iniciar o App e preparar o service worker.
* `App.css` CSS para os componentes da aplicação.
* `App.js` O componente inicial da aplicação frontend em react em si.
* `logo.svg` Um logo para sua página :)
* `index.css` E um CSS padrão para os elementos padrões HTML.

Vamos estudar os principais arquivos a seguir.

## index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

Primeira diferença esta na estrutura de acesso aos módulos. O NodeJS faz uso do `require`, uma função disponibilziada pela biblioteca `commonjs`, incorporada ao nodejs para realizar o acesso aos arquivos do sistema. O uso de `import` é nativo do ECMAScript 6 e é responsável pelo acesso aos outros módulos do sistema.

O `import` foi usado para acessar as bibliotecas do React, bem como o próprio CSS e funções do `serviceWorker.js`.

É possível observar também que o `render` do `ReactDOM` está sendo executado. Ele irá renderizar o componente `App`. Observe que esse componente é também um elemento virtual de HTML (`<App>`).

Quando um componente é criado ele é criado, um elemento associado passa a existir e pode ser utilizado para a composição de elementos.

Digamos que existam os componentes de `NavBar`, `Content` e `Footer`. Poderiamos estruturar tais componentes usando a estrutura:

```jsx
<div>
  <NavBar />
  <Content />
  <Footer />
</div>
```

Se os 3 componentes estiverem importados, eles serão renderizados (HTML associado a tais componentes é criado e exibido pelo browser).

## index.css

```css
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Ox,
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}
```

O CSS é o arquivo de estilização da sua página. O nosso `index.css` identifica elementos e as propriedades de estilo de tais elementos.

O HTML tem o elemento `<body>` que define o corpo do HTML. O `index.css` está definindo as configurações do body e também do elemento code.

Assim, tantos os elementos `<body>` e `<code>` terão seus estilos definidos pelo `index.css`.

## App.js

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
}

export default App;
```

O `App.js` define o seu primeiro componente React. Ao criar um componente react estamos definindo um elemento.

É possível observar a definição de uma classe no div, o uso do logo, e do code. Uma classe é definida através de um atributo, ou seja, um qualificador do elemento (no caso, `className`).

## App.css

```css
.App {
  text-align: center;
}

.App-logo {
  animation: App-logo-spin infinite 20s linear;
  height: 40vmin;
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

.App-link {
  color: #61dafb;
}

@keyframes App-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

```

É possível observar no `App.css` a definição de estilos para as classes `App`, `App-logo`, etc. Uma classe é representada no CSS pelo `.` no início do nome da classe.

Para os curiosos, o `@keyframes` define uma animação através de uma transformação (rotação).

# Primeiro componente

# Props e estado

# Novas funcionalidades ao componente

