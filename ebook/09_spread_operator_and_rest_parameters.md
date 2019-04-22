# Capítulo 9: Operador spread e parâmetros rest

## O operador spread

De acordo com o MDN:
> A sintaxe spread permite um iterável tal como a de uma expressão array ou string ser expandida em locais onde zero ou mais argumentos (para chamada de função) ou elementos (para arrays literais) são esperados, ou uma expressão objeto ser expandida em locais onde zero ou mais chaves-valores (para objetos literais) são esperados.

&nbsp;

### Combinar arrays

``` js
const vegetais = ["tomate", "pepino", "feijão"];
const carnes = ["porco", "bife", "galinha"];

const menu = [...vegetais, "pasta", ...alimentos];
console.log(menu);
// Array [ "tomate", "pepino", "feijão", "pasta", "porco", "bife", "galinha" ]
```

Os `...` compõem a sintaxe spread, e ele nos permite obter todos os valores individuais dos arrays vegetais e carnes e os colocam dentro do array menu no mesmo momento em que adicionam novos itens entre eles.

&nbsp;

### Copy arrays

The spread syntax is very helpful if we want to create a copy of an array.


``` js
const veggie = ["tomato","cucumber","beans"];
const newVeggie = veggie;

// this may seem like we created a copy of the veggie array but look now

veggie.push("peas");
console.log(veggie);
// Array [ "tomato", "cucumber", "beans", "peas" ]

console.log(newVeggie);
// Array [ "tomato", "cucumber", "beans", "peas" ]
```

Our new array also changed, but why? Because we did not actually create a copy but we just referenced our old array in the new one.
This is how we would usually make a copy of an array in ES5 and earlier.

``` js
const veggie = ["tomato","cucumber","beans"];
const newVeggie = [].concat(veggie);
// we created an empty array and put the values of the old array inside of it
```

And this is how we would do the same using the spread syntax:

``` js
const veggie = ["tomato","cucumber","beans"];
const newVeggie = [...veggie];
```

&nbsp;

### Spread into a function

``` js
// OLD WAY
function doStuff (x, y, z) {
  console.log(x + y + z);
 }
var args = [0, 1, 2];

// Call the function, passing args
doStuff.apply(null, args);

// USE THE SPREAD SYNTAX

doStuff(...args);
// 3 (0+1+2);
console.log(args);
// Array [ 0, 1, 2 ]
```

We can replace the `.apply()` syntax and just use the spread operator.

Let's look at another example:


``` js
const name = ["Alberto", "Montalesi"];

function greet(first, last) {
  console.log(`Hello ${first} ${last}`);
}

greet(...name);
// Hello Alberto Montalesi
```

The two values of the array are automatically assigned to the two arguments of our function.

What if we provide more values than arguments?

``` js
const name = ["Jon", "Paul", "Jones"];

function greet(first, last) {
  console.log(`Hello ${first} ${last}`);
}
greet(...name);
// Hello Jon Paul
```

We provided 3 values inside our array but only have 2 arguments in our function therefore the last one is left out.

&nbsp;

### Spread in Object Literals (ES 2018 / ES9)

This feature is not part of ES6, but as we are already discussing this topic, it is worth mentioning that ES2018 will introduce the Spread operator for Objects.
Let's look at an example:

``` js
let person = {
  name : "Alberto",
  surname: "Montalesi",
  age: 25,
}

let clone = {...person};
console.log(clone);
// Object { name: "Alberto", surname: "Montalesi", age: 25 }
```

You can read more about ES2018 in Chapter 20.

---
&nbsp;

## The Rest parameter

The rest syntax looks exactly the same as the spread, 3 dots `...` but it is quite the opposite of it. Spread expands an array, while rest condenses multiple elements into a single one.

```js

const runners = ["Tom", "Paul", "Mark", "Luke"];
const [first,second,...losers] = runners;

console.log(...losers);
// Mark Luke
```

We stored the first two values inside the `const` first and second and whatever was left we put it inside losers using the rest parameter.
