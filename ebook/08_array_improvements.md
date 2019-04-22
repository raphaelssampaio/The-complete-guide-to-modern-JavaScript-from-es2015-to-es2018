# Chapter 8: Melhorias no Array

## `Array.from()`

`Array.from()` é o primeiro de muitos novos métodos que foram apresentados no ES6.

Ele vai tornar algo **arrayzável**, isso significa que se algo parece com um array, mas não é, ele vai transformar em um array real.

``` js
// nosso html
<div class="fruits">
  <p> Maçã </p>
  <p> Banana </p>
  <p> Laranja </p>
</div>

const frutas = document.querySelectorAll(".frutas p");
const frutaArray = Array.from(frutas);

console.log(frutas);
//vai nos retornar um array contendo 3 tags p [p,p,p];

//tendo em vista que agora nós estamos lidando com um array nós podemos usar o map
const nomeFrutas = frutaArray.map(fruta => fruta.textContent);

console.log(nomeFrutas);
// ["Maçã", "Banana", "Laranja"]
```

Nós podemos simplificar também:

```js
const frutas = Array.from(document.querySelectorAll(".frutas p"));
const nomeFrutas = frutas.map(fruta => fruta.textContent);

console.log(nomeFrutas);
// ["Maçã", "Banana", "Laranja"]
```

Agora nós transformamos **frutas** em um array real, o que significa que nós podemos usar todo tipo de método tal como `map` nela.

`Array.from()` também tem um segundo argumento, uma função `map`. Então nós podemos escrever:

``` js
const frutas = document.querySelectorAll(".frutas p");
const frutaArray = Array.from(frutas, fruta => {
  console.log(fruta);
  // <p> Maçã </p>
  // <p> Banana </p>
  // <p> Laranja </p>
  return fruta.textContent;
  // nós somente queremos obter o conteúdo, não toda a tag
});
console.log(frutaArray);
// ["Maçã", "Banana", "Laranja"]
```

&nbsp;

## `Array.of()`

`Array.of()` vai criar um array com todos os argumentos que nós passamos a ele.

```js
const digitos = Array.of(1,2,3,4,5);
console.log(digitos);

// Array [ 1, 2, 3, 4, 5];
```

&nbsp;

## `Array.find()`

`Array.find()` retorna o valor do primeiro elemento no array que satisfaz a função de teste fornecida. Caso contrário `undefined` é retornado.

Ele pode ser útil em intâncias onde nós temos um arquivo json, talvez vindo de uma API do instagram ou algo parecido e nós queremos obter um post específico com um código específico que o identifica.

Vamos ver um exemplo simples para notar como `Array.find()` trabalha.

``` js
const array = [1,2,3,4,5];

let encontrado = array.find( e => e > 3 );
console.log(encontrado);
// 4
```

Como mencionamos, ele vai retornar o **primeiro elemento** que se encaixa em nossa condição, esse é o motivo de nós termos obtido **4** e não **5**.

&nbsp;

## `Array.findIndex()`

`Array.findIndex()` vai retornar o *index* do elemento que se encaixa em nossa condição.

``` js
const saudacoes = ["ola","oi","tchau tchau","adeus","oi"];

let indexEncontrado = greetings.findIndex(e => e === "oi");
console.log(indexEncontrado);
// 1
```

Mais uma vez, somente o index do **primeiro elemento** que se encaixa na nossa condição é retornado.

&nbsp;

## `Array.some()` & `Array.every()`

Estou agrupando esses dois juntos porque seus usos são auto-explitivos: `.some()` vai procurar se existe algum item que se encaixa na nossa condição e vai parar quando encontrar o primeiro, `.every()` vai checar todos os itens que se encaixam na condição informada.

```js
const array = [1,2,3,4,5,6,1,2,3,1];

let arraySome = array.some( e => e > 2);
console.log(arraySome);
// true

let arrayEvery = array.every(e => e > 2);
console.log(arrayEvery);
// false
```

Simplificando, a primeira condição é verdadeira, porque existe **algum** elemento maior do que 2, mas na segunda é falsa porque **nem todos os elementos** são maiores que 2.
