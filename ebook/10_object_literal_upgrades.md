# Capítulo 10: Upgrades no Objeto literal

Neste artigo nós veremos os vários upgrades trazidos pelo ES6 para a notação do Objeto literal.

&nbsp;

## Desconstruindo variáveis em chaves e valores

Esta é a nossa situação inicial:

``` js
const nome = "Alberto";
const sobrenome = "Montalesi";
const idade = 25;
const nacionalidade = "Italiano";
```

Agora se nós quisermos criar um objeto literal é assim que nós normalmente faríamos:

```js
const pessoa = {
  nome: nome,
  sobrenome: sobrenome,
  idade: idade,
  nacionalidade: nacionalidade,
}
```

No ES6 nós podemos simplesmente fazer assim:

```js
const pessoa = {
  nome,
  sobrenome,
  idade,
  nacionalidade,
}
console.log(pessoa);
// {nome: "Alberto", sobrenome: "Montalesi", idade: 25, nacionalidade: "Italiano"}
```

Como nossa `const` tem o mesmo nome da propriedade que estamos usando, nós podemos reduzir bastante a nossa escrita.

&nbsp;

## Adicionar funções para nossos Objetos

Vamos ver um exemplo do ES5:

``` js
const pessoa = {
  nome: "Alberto",
  saude: function(){
    console.log("Olá");
  },
}
```

Se nós quisermos adicionar uma função ao nosso Objeto nós teríamos que usar a palavra-chave `function`. No ES6 facilitaram para nós, veja:

``` js
const pessoa = {
  nome: "Alberto",
  saude(){
    console.log("Olá");
  },
}

pessoa.saude();
// Olá;
```

Sem `function`, é mais curto e faz a mesma coisa.

**Lembre-se** que **arrow functions** são anônimas, veja este exemplo:

``` js
// arrow functions são anônimas, neste caso você precisa ter a chave
const pessoa1 = {
  () => console.log("Olá"),
};

const pessoa2 = {
  saude: () => console.log("Olá"),
}
```

&nbsp;

## Definir dinamicamente as propriedades de um Objeto

O que se segue é como nós difiniríamos dinamicamente as propriedades de um Objeto no ES5:

``` js
var nome = "nome";
// criar um objeto vazio
var pessoa = {}
// atualizar o objeto
pessoa[nome] = "Alberto";
console.log(pessoa.nome);
// Alberto
```

Primeiro nós criamos o Objeto e então nós o modificamos.

No ES6 nós podemos fazer as duas coisas ao mesmo tempo, veja:

``` js
const nome = "nome";
const pessoa = {
  [nome]:"Alberto",
};
console.log(pessoa.nome);
// Alberto
```
