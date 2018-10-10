O React em si define uma aplicação que é executada pelo browser do cliente.

Pela arquitetura proposta do react cada componente tem um ciclo de vida bem definido, ou seja, um comportamento próprio para sua preparação, criação e eliminação.

Cada componente basicamente opera em 3 etapas bem definidas:

* Montagem (Mount)
* Atualização (Update)
* Remoção (Umount)

Ao ser invocado (ao usar o elemento do componente em questão) o mesmo é criado. O **construtor** do componente é invocado, de forma que possa ser utilizado. Um componente é criado para cada uma das invocações existentes. Depois de criado, o componente é renderizado (render). O processo de renderização envolve a criação de um DOM (Document Object Model) e a associação desse DOM com a página do usuário de fato.

Terminada a fase de montagem, o componente é atualizado quando são detectadas alterações em seu estado. Isso pode acontecer tanto por uma alteração no estado do componente (`setState`) como pela inserção de novos `propos` no componente.

Por fim, quando o componente não deve mais ser associado a página do usuário o mesmo é removido.

Todo o processo e controle de vida de um componente React é executado de forma automática pelo framework. O desenvolvedor pode, no entanto, inserir métodos no componente que serão invocados durante o ciclo de vida do componente. São esses métodos:

* Montagem: `componentDidMount`
* Atualização: `componentDidUpdate`
* Remoção: `componentWillUnmount`

Dan Abramov organizou uma [imagem para o ciclo de vida de componentes](https://twitter.com/dan_abramov/status/981712092611989509).

![Ciclo de vida do componente](https://raw.githubusercontent.com/matheusgr/devweb/master/lifecycle.jpeg)

# Consumindo uma API

A preparação do componente existe para preparar o documento que irá contê-lo e ser renderizado.

Uma vez preparado, o documento está apto a ser preenchido. Podemos fazer uso do gancho `componentDidMount` para capturar dados do backend e colocá-los no nosso componente.

Veja o exemplo abaixo.

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

    componentDidMount() {
        fetch('https://api.example.com')
            .then(response => response.json())
            .then(data => this.setState({ data }));
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

TODO

https://www.robinwieruch.de/react-pass-props-to-component/

https://www.robinwieruch.de/react-fetching-data/
