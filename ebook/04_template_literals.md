# Capítulo 4: Template literal

Antes do ES6 eles eram chamados de *template strings*, agora nós os chamamos de *template literal*. Vamos ter que ver o que mudou na maneira de interpolar strings no ES6.

&nbsp;

## Interpolando strings

No ES5 nós costumávamos escrever isso, a fim de interpolar strings:

``` javascript
var name = "Alberto";
var greeting = 'Olá meu nome é ' + name;

console.log(greeting);
// Olá meu nome é Alberto
```

No ES6 nós podemos usar o símbolo da crase para tornar nossa vida mais fácil.

``` javascript
let name  = "Alberto";
const greeting = `Olá meu nome é ${name}`;

console.log(greeting);
// Olá meu nome é Alberto
```

&nbsp;

## Interpolação de expressões

No ES5 nós costumávamos escrever assim:

``` javascript
var a = 1;
var b = 10;
console.log('1 * 10 is ' + ( a * b));
// 1 * 10 é 10

```

No ES6 nós podemos usar o símbolo da crase para reduzir nossa digitação:

``` javascript
var a = 1;
var b = 10;
console.log(`1 * 10 é ${a * b}`);
// 1 * 10 é 10
```

&nbsp;

## Criar fragmentos HTML

No ES5 nós costumávamos escrever strings multi-linhas:


``` javascript
// Nós temos que incluir uma barra invertida em cada linha
var text = "Olá, \
meu nome é Alberto \
como vai você?\ "
```

No ES6 nós simplesmente temos que envolver tudo dentro de colchetes, sem mais barras invertidas em cada linha

``` javascript 
const content = `Olá,
meu nome é Alberto
como vai você?`;
```

&nbsp;

## Aninhamento de templates

É muito fácil aninhar um template dentro de outro, dessa forma:

``` js
const markup = `
<ul>
  ${people.map(person => `<li>  ${person.name}</li>`)}
</ul>
`;
```

&nbsp;

## Adicionar um operador ternário

Nós podemos facilmente adicionar alguma lógica dentro do nosso template de string usando um operador ternário:

``` js
// criando um artista com nome e idade
const artist = {
  name: "Bon Jovi",
  age: 56,
};

// somente se o objeto artista tiver uma propriedade song então nós adicionamos no nosso parágrafo, caso contrário não retormanos nada
const text = `
  <div>
    <p>  ${artist.name} tem ${artist.age} de idade ${artist.song ? `e escreveu a música ${artist.song}` : '' }
    </p>
  </div>
`
```

&nbsp;

## Passando uma função dentro de um template literal

Do mesmo modo do exemplo acima, se nós quisermos, nós podemos passar a função dentro de um template literal.

``` js
const alimentos = {
  carne: "costela de porco",
  vegetais: "salada",
  fruta: "maçã",
  outros: ['cogumelos', 'macarrão instântaneo', 'sopa instantânea'],
}

// essa função mapeará cada valor individual dos nossos alimentos
function alimentosLista(outros) {
  return `
    <p> 
      ${outros.map( outro => ` <span> ${outro}</span>`).join(' ')}
    </p>
  `;
}

// mostrar todos os nossos alimentos em uma tag p, o último incluirá todos eles do array **outros**
const markup = `
  <div>
    <p> ${alimentos.refeicao} </p>
    <p> ${alimentos.vegetais} </p>
    <p> ${alimentos.fruta} </p>
    <p>${alimentosLista(alimentos.outros)} </p>
  <div>
`
```

&nbsp;

## Template literais marcados

Marcando uma função para um template literal nós podemos rodar o template literal por entre a função, fornecendo-a tudo que está dentro do template.

A maneira de como isso funciona é muito simples: você só pega o nome da sua função e coloca na frente de um template que você quer que rode contra.

 ```js
let pessoa = "Alberto";
let idade = 25;

function minhaTag(strings,pessoaNome,pessoaIdade){
  let str = strings[1];
  let idadeStr;

  pessoaIdade > 50 ? idadeStr = "vovô" : ageStr = "jovem";

  return pessoaNome + str + idadeStr;
}

let sentenca = minhaTag`${pessoa} é um ${idade}`;
console.log(sentenca);
// Alberto é um jovem
```

Nós capturamos o valor da variável idade e usamos um operador ternário para decidir o que printar.
`strings` tomará todas as strings da nossa sentença `let` enquanto os outros parâmetros manterão as variáveis.

&nbsp;

Para aprender mais sobre os casos de uso dos *template literais* veja [este artigo](https://codeburst.io/javascript-es6-tagged-template-literals-a45c26e54761).
