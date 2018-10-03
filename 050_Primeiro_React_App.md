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

# Primeiro componente e propriedades

Vamos criar nosso primeiro componente. Queremos poder customizar e expandir a funcionalidade do elemento `<code>` de HTML. Para isso vamos criar o nosso primeiro componente próprio, o `Code`.

```javascript
import React, { Component } from 'react';

class Code extends Component {
  render() {
    return (
      <code>
        {this.props.children}
      </code>
    );
  }
}

export default Code;
```

E adaptar nossa aplicação para fazer uso do componente criado:

```javacript
//...
import Code from './Code';
//...
            Edit <Code>src/App.js</Code> and save to reload.
//...
```

É possível observar no componente `Code` o uso de `this.props`. Este atributo representa um conjunto de propriedades que é passado ao componente e que o mesmo pode fazer uso para enriquecer sua aplicação. No exemplo, fizemos uso do `children` que representa tudo aquilo que foi passado dentro da invocação do elemento `Code` pelo componente do `App`. No nosso exemplo, o `children` é o texto `src/App.js`.

Você pode passar propriedades a partir de atributos:

```jsx
Edit <Code plus="*">src/App.js</Code> and save to reload.
```

E usar tais atributos no dicionário `props`:

```jsx
<code>
  {this.props.plus} {this.props.children} {this.props.plus}
</code>
```

# Novas funcionalidades ao componente

Vamos agora expandir o componente `Code`! Vamos criar um construtor para o componente (`constructor()`) que prepara o estado do componente (`this.state`) e uma função (`this.handleClick`).

Observe o comportamento do handleClick. Nele, o estado do objeto é atualizado. O mecanismo do `setState` permite que o react possa reagir a alterações do estado do componente de forma que ele próprio possa ser atualizado de acordo com a atualização.

```javascript
import React, { Component } from 'react';

class Code extends Component {
    constructor() {
        super();
        this.state = {
            bold: true
        };
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick(e) {
        this.setState({
            bold: !this.state.bold
        });
    }

    render() {
        return (
            <code onClick={this.handleClick}>
                {this.state.bold ? this.props.plus : ""} {this.props.children}
            </code>
        );
    }
}

export default Code;
```

# E agora?

Comece definindo os principais componentes do seu App. Foque no componente da principal funcionalidade do seu sistema.

Não comece com Home, Login, Dashboard... pense em qual é a coisa mais importante da sua aplicação. E prepare essa tela.

Defina um componente principal para conter os elementos existentes nessa página principal. E crie 2 ou 3 níveis de componentes até chegar a um componente que possa ser trabalhado.

Como exemplo, no dirlididi, nós temos a resolução de um problema como a principal tela da aplicação. Ela é composta dos componentes:

* navbar
* problema
* ide
* submissões

Cada um desses componentes podem ser descritos em outros componentes. O problema pode ser descrito pelos componentes de título, chave, descrição e exemplos.

Considerando essa estruturação, poderia-se começar pelo componente de título, depois chave, depois descrição, depois de exemplos de entrada e saída.
