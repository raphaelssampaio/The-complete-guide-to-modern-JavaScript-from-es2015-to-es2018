# Chapter 2: Arrow functions

## O que são Arrow Functions?

ES6 apresentou os fat-arrows (`=>`) como um jeito de declarar funções.
Esse é o jeito que normalmente nós declaramos uma função no ES5:

``` javascript
var greeting = function(name) {
  return "Olá " + name;
}
```

A nova sintase com um fat-arrow é assim:

``` javascript
const greeting = (name) => {
  return `Olá ${name}`;
}
```

Nós podemos ir além, se nós só temos um parâmetro nós podemos tirar o parêntesis e escrever:

``` javascript
const greeting = name => {
  return `Olá ${name}`;
}
```

Se nós não temos parâmetros nós precisamos escrever os parêntesis vazios assim:

``` javascript
const greeting = () => {
  return "Olá";
}
```

&nbsp;

## Retorno implícito

Com as arrow functions nós podemos ignorar os retornos explícitos e retornar algo assim:

``` javascript
const greeting = name => `Olá ${name}`;
```

Vamos dizer que nós queremos o retorno implícito de um **objeto literal**, nós faríamos algo assim:

``` javascript
const corrida = "100m rasos";
const corredores = [ "Usain Bolt", "Justin Gatlin", "Asafa Powell" ];

const vencedor = corredores.map((corredor, i) =>  ({ name: corredor, corrida, colocacao: i + 1}));

console.log(vencedor);
// {name: "Usain Bolt", corrida: "100m rasos", colocacao: 1}
// {name: "Justin Gatlin", race: "100m rasos", colocacao: 2}
// {name: "Asafa Powell", race: "100m rasos", colocacao: 3}
```

Para dizer ao JavaScript que o que está dentro dos colchetes é um **objeto literal** que nós queremos retornar implicitamente, nós precisamos colocar tudo dentro de parêntesis.

Escrever `corrida` ou `corrida:corrida` é a mesma coisa.

&nbsp;

## Arrow functions são anônimas

Como você pode ver nos exemplos anteriores, arrow functions são **anônimas**.

Se nós queremos ter um nome para referenciá-los nós podemos vinculá-los a uma variável:

``` javascript
const greeting = name => `Olá ${name}`;

greeting("Tom");
```


&nbsp;

## Arrow functions e a palavra-chave `this`

Nós precisamos ser cuidadosos quando estamos usando arrow functions em conjunto com a palavra-chave `this`, porque eles têm comportamento diferente das funções normais.

Quando você usa um arrow function, a palavra-chave `this` é herdado do escopo pai.

Isso pode ser útil em casos como este que se segue:

``` javascript 
// prenda nossa div com um class box
const box = document.querySelector(".box");
// ouça um evento de click
box.addEventListener("click", function() {
  // altere a classe oppening na div
  this.classList.toggle("opening");
  setTimeout(function(){
    // tente alterar a classe novamente
    this.classList.toggle("open");
    });
});
```


O problema nesse caso é que o primeiro `this` está vinculado a `const` mas no segundo caso, dentro do `setTimeout`, será setado ao objeto `Window`, lançando este erro:

``` javascript
Uncaught TypeError: cannot read property "toggle" of undefined 
```

Desde que nós sabemos que as **arrow functions** herdam o valor de `this`do escopo pai, nós podemos reescrever nossas funções dessa forma:

``` javascript
// prenda nossa div com um class box
const box = document.querySelector(".box");
// ouça um evento de click
box.addEventListener("click", function () {
  // altere a classe oppening na div
  this.classList.toggle("opening");
  setTimeout(() => {
    // tente alterar a classe novamente
    this.classList.toggle("open");
   });
});
```

Aqui, o segundo `this` será herdado do seu pai, e será setado a uma `const`.

&nbsp;

## Quando você deve evitar arrow functions

Usando o que nós conhecemos sobre a herança da palavra-chave `this` nós podemos definir algumas instâncias onde você **não** deve usar arrow functions.

Os próximos 2 exemplos todos mostram quando ser criterioso usando `this` dentro das arrows.

``` javascript
const button = document.querySelector("btn");
button.addEventListener("click", () => {
  // error: *this* se refere ao Objeto `Window`
  this.classList.toggle("on");
})
```

``` javascript
const person = {
  age: 10,
  grow: () => {
    // error: *this* se refere ao Objeto `Window`
    this.age++;
  }
}
```

Uma outra diferença entre Arrow functions e funções normais é o acesso aos `arguments`.

```javascript
const orderRunners = () => {
  const runners = Array.from(arguments);
  return runners.map((runner, i) => {
    return `#{runner} was number #{i +1}`;
   })
}
```

Este código retornará:

``` javascript
ReferenceError: arguments is not defined
```

Para acessar todos os argumentos de um array, use a notação antiga das funções, ou a sintaxe splat. O nome do parâmetro não importa (aqui ele é chamado de args para reduzir a confusão com `arguments`).

```javascript
const orderRunners = (...args) => {
  const runners = Array.from(args);
  console.log(runners);
  return runners.map((runner, i) => {
    return `#{runner} was number #{i +1}`;
  })
}
```
