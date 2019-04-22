# Capítulo 6: Desestruturação

MDN define **desestruturação** dessa forma:

> A sintaxe da atribuição de desestruturação é uma expressão JavaScript que torna possível o desempacotamento de valores de um array, ou propriedades de objetos, dentro de variáveis distintas.

Vamos começar com **desestruturação de objetos** primeiro.

&nbsp;

## Desestruturação de objetos

Para criar variáveis de um objeto nós costumamos fazer o seguinte:

```js
var pessoa  = {
  primeiro: "Alberto",
  ultimo: "Montalesi"
}

var primeiro = pessoa.primeiro;
var ultimo = pessoa.ultimo;
```

No ES6 nós podemos agora escrever assim:

```js
const pessoa = {
  primeiro: "Alberto",
  ultimo: "Montalesi"
}

const { primeiro, ultimo } = pessoa;
``` 

Já que nossa `const` tem o mesmo nome das propriedades que nós queremos pegar, nós não temos mais que especificar `pessoa.nome` e `pessoa.ultimo`.

O mesmo se aplica quando nós temos dados aninhados, como o que podemos obter de uma API

```js
const pessoa = {
  nome: "Alberto",
  ultimo: "Montalesi",
  links:{
    social: {
      facebook: "https://www.facebook.com/alberto.montalesi",
    },
    website: "http://albertomontalesi.github.io/"
  }
}

const { facebook } = pessoa.links.social;
```

Nós não limitamos o nome da nossa variável ao mesmo nome das propriedades do objeto, nós podemos renomeá-la dessa forma:


```js
const { facebook:fb } = ppessoa.links.social;
// nós renomeamos a variável para *fb* 
```

Nós podemos também passar **valores padrões** como esse:

```js
const { facebook:fb = "https://www.facebook.com"} = pessoa.links.social;
// nós renomeamos a variável para *fb* e também setamos um valor padrão para ela
```


---
&nbsp;


## Desestruturação de arrays

A primeira diferença que nós identificamos quando **desestruturamos arrays** é que nós iremos usar `[]` e não `{}`.

```js
const pessoa = ["Alberto","Montalesi",25];
const [nome,sobrenome,idade] = pessoa;
```

E se o número de variáveis que nós criamos for menor do que os elementos no array?

```js
const pessoa = ["Alberto","Montalesi",25];
// nós omitimos a idade, nós não queremos ela
const [nome,sobrenome] = pessoa;
// o valor da idade não será passado para nenhuma variável.
console.log(nome,sobrenome);
// Alberto Montalesi
```

Vamos falar que nós queremos obter todos os outros valores restantes, nós podemos usar o **operador rest**:

```js
const pessoa = ["Alberto", "Montalesi", "pizza", "sorvete", "bolo de queijo"];
// nós usamos o **operador rest** para obter todos os valores restantes
const [nome,sobrenome,...comida] = pessoa ;
console.log(comida);
// Array [ "pizza", "sorvete", "bolo de queijo" ]
```

&nbsp;

## Trocando variáveis com a desestruturação

A atribuição de desestruturação faz com o que a troca de variáveis seja **extremamente fácil**, veja o seguinte exemplo:

```js
let comFome = "sim";
let cheio = "nao";
// depois de comer nós não sentimos mais fome, nós estamos cheios, valos trocar os valores

[comFome, cheio] = [cheio, comFome];
console.log(comFome,cheio);
// nao sim
```

Não é mais fácil do que isso trocar valores.

&nbsp;

## Desestruturação de funções

```js
function contaTotal({ total, taxa = 0.1 }) {
  return total + (total * taxa);
}

// como você vê, uma vez que usamos os mesmos valores, nós não temos que passar os argumentos na mesma ordem em que nós declaramos a função
// nós estamos também substituindo o valor padrão que nós setamos para a taxa
const conta = contaTotal({ taxa: 0.15, total: 150});
```
