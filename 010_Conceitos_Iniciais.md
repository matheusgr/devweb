# Conceitos Iniciais

A triade HTML / HTTP / JavaScript define a internet.

* HTML: Formato de hiper texto através de uma linguagem com marcação
* HTTP: Protocolo para transferência de hipertexto
* Javascript: Linguagem de programação utilizada bastante para manipulação do documento web

No início é importante se habituar e acostumar com o uso de HTML como um documento. É preciso entender seu formato, principais elementos e como este documento é renderizado no navegador. Por exemplo, considere o HTML abaixo:

```html
<body>
  <h1>Introdução</h1>
  <div>
    Texto inicial da disciplina. É <b>muito</b> importante entender os conceitos iniciais.
  </div>
  <img src="smiley.gif" />
  <a href="www.google.com">Goooogle</a>
</body>
```

O HTML apresentado é estruturado hierarquicamente. O `body` é o elemento que encapsula todo corpo do documento. Observe que se forma uma relação hierarquica entre as entidades, por exemplo, dentor do `div` existe um texto (conteúdo) que internamente apresenta um elemnto `<b>` que torna um texto em negrito.

Por sua vez, os elementos podem ter propriedades como é o caso da imagem (`<img>`) e do link (`<a>`).

Estes elementos são identificados por `tags` ou marcações e definem a estrutura básica do HTML. No browser, cada HTML é interpretado para gerar um modelo de um documento (`DOM`) que nad amais é do que uma árvore navegável. O primeiro elemento da DOM é o `document` em si e, a partir dele, é possível acessar os demais elementos filhos. A `DOM`, combinada com a `folha de estilos` (descreve o formato dos elementos) são combinadas para gerar um desenho de tela a ser renderizado (transformado em elementos visuais) para o usuário.

Nesta disciplina você começa pensando no HTML da sua principal funcionalidade. É importante começar com a principal funcionalidade pois assim você pode ter uma idéia precisa do que pode, ou não, ser feito. E é a partir dessa tela inicial que toda a aplicação será construída.

## Dicas - VSCode

Para as pessoas que usam VSCode, recomendamos as seguintes extensões:

* Bracket Pair Colorizer
* ES7 React/Redux/GraphQL/React-Native snippets
* Live Server (Cria um servidor local para ver páginas HTML)
* Live Share (Programação compartilhada)
* Node.js Modules Intellisense

E as seguintes configurações:

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

## Projeto

### Características

* Deve seguir algumas regras para o seu bem-estar:
  * Deve te deixar feliz ao fazer!
  * Deve ser divertido fazer!
  * Deve te dar orgulho de fazer!
  * Deve ser algo que você vai aguentar fazer por 1 período inteiro!

### Obrigações

* Público
* Individual (dois alunos podem ter o mesmo tema, mas cada um deve tentar dar a ‘sua cara’/personalização)
* Algo que vc possa terminar (não precisa ser completo, mas deve ser algo utilizável no final da disciplina)
* Algo que você nunca fez antes

### Sugestões

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

## Organização de Código

1. Crie UM repositório no github/lab/bitbucket para o seu projeto (só precisa fazer essa criação quando definir o que irá fazer, até lá pode ir para o passo 3)
   1. Você pode usar qualquer canto que hospede projetos GIT, mas o repositório deve permitir o cadastro de issues por qualquer usuário
   1. Um diretório do seu repositório terá o backend
   1. Outro diretório será usado para o frontend (interface) da aplicação
   1. Público
   1. Linguagem: Javascript/Typescript

## Tecnologias

O projeto será dividido em dois componentes: **backend** que é responsável pelos dados e lógica de negócio e o **frontend**, responsável pela interface com o usuário. No frontend, preocupe-se apenas com o HTML por enquanto.

Para o backend, usaremos 3 tecnologias:

1. NodeJS: O interpretador javascript que utilizaremos na primeira parte da disciplina.
1. Express: Framework para definição de rotas e serviços HTTP (protocolo de acesso web)
1. NPM/yarn: Gerente de pacotes javascript para o nodejs