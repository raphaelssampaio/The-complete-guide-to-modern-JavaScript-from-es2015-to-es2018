# Capítulo 7: Iterações e repetições

ES6 apresentou um novo tipo de estrutura de repetição, o `for of`.

&nbsp;

## A estrutura de repetição `for of`

### Iteração de um array

Normalmente nós iteraríamos usando algo assim:

``` js
var frutas = ['maçã','banana','laranja'];
for (var i = 0; i < frutas.length; i++){
  console.log(frutas[i]);
}
// maçã
// banana
// laranja
```

Veja como nós podemos conseguir a mesma coisa usando o `for of`:

``` js
const fruits = ['maçã','banana','laranja'];
for(const fruta of frutas){
  console.log(fruta);
}
// maçã
// banana
// laranja
```

&nbsp;

### Iteração de um objeto

Objetos são **não iteraveis** então como nós faremos para eles iterarem?
Primeiro nós temos que obter todos os valores do objeto usando `Object.keys()` ou `Object.entries()` apresentado no ES6.

```js
const carro = {
  fabricante: "BMW",
  cor: "red",
  ano : "2010",
}

for (const prop of Object.keys(carro)){
  const valor = carro[prop];
  console.log(valor,prop);
}
// BMW fabricante
// red cor
// 2010 ano
```

&nbsp;

## A estrutura de repetição `for in`

Embora não seja uma nova estrutura de repetição do ES6, vamos ver o `for in` para entender qual a diferença quando comparado ao `for of`.

O `for in` é um pouco diferente pois ele vai iterar sobre todas as [propriedades enumeradas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties) de um objeto e não na ordem particular.

Portanto, sugere-se não adicionar, modificar ou excluir propriedades do objeto durante a iteração já que não há garantia de que eles serão visitados, ou se eles serão visitados antes de serem modificados.

```js
const carro = {
  fabricante: "BMW",
  cor: "red",
  ano : "2010",
}
for (const prop in carro){
  console.log(prop, carro[prop]);
}
// fabricante BMW
// cor red
// ano 2010
```

&nbsp;

## A diferença entre `for of` e `for in`

A primeira diferença que nós podemos ver esta neste exemplo:

```js
let lista = [4, 5, 6];

// for...in retorna uma lista de chaves
for (let i in lista) {
   console.log(i); // "0", "1", "2",
}


// for ...of retorna os valores
for (let i of lista) {
   console.log(i); // "4", "5", "6"
}
```

`for in` retornará uma lista de chaves enquanto o `for of` retornará uma lista de vlores da propriedade numérica do objeto que está sendo iterado.

Uma outra diferença é que nós **podemos** parar um `for of` mas não podemos fazer o mesmo com um `for in`.

```js
const digitos = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digito of digitos) {
  if (digito % 2 === 0) {
    continue;
  }
  console.log(digito);
}
// 1 3 5 7 9
```

A última importante diferença que eu quero falar é que o `for in` vai iterar sobre uma nova propriedade adicionada ao objeto.

```js
const fruta = ["maçã","banana", "laranja"];

fruta.comer = "nhame nhame";

for (const prop of fruta){
  console.log(prop);
}
// maçã
// banana
// laranja

for (const prop in fruta){
  console.log(fruta[prop]);
}

// maçã
// banana
// laranja
// nhame nhame
```
