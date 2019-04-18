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

## Implicitly return

With arrow functions we can skip the explicit return and return like this:

``` javascript
const greeting = name => `hello ${name}`;
```

Let's say we want to implicitly return an **object literal**, we would do like this:

``` javascript
const race = "100m dash";
const runners = [ "Usain Bolt", "Justin Gatlin", "Asafa Powell" ];

const winner = runners.map((runner, i) =>  ({ name: runner, race, place: i + 1}));

console.log(winner);
// {name: "Usain Bolt", race: "100m dash", place: 1}
// {name: "Justin Gatlin", race: "100m dash", place: 2}
// {name: "Asafa Powell", race: "100m dash", place: 3}
```

To tell JavaScript that what's inside the curly braces is an **object literal** that we want to implicitly return, we need to wrap everything inside parenthesis.

Writing `race` or `race: race` is the same.

&nbsp;

## Arrow functions are anonymous

As you can see from the previous examples, arrow functions are **anonymous**.

If we want to have a name to reference them we can bind them to a variable:

``` javascript
const greeting = name => `hello ${name}`;

greeting("Tom");
```


&nbsp;

## Arrow function and the `this` keyword

You need to be careful when using arrow functions in conjunction with the `this` keyword, as they behave differently from normal functions.

When you use an arrow function, the `this` keyword is inherited from the parent scope.

This can be useful in cases like this one:

``` javascript 
// grab our div with class box
const box = document.querySelector(".box");
// listen for a click event 
box.addEventListener("click", function() {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(function(){
    // try to toggle again the class
    this.classList.toggle("open");
    });
});
```


The problem in this case is that the first `this` is bound to the `const` box but the second one, inside the `setTimeout`, will be set to the `Window` object, trowing this error:

``` javascript
Uncaught TypeError: cannot read property "toggle" of undefined 
```

Since we know that **arrow functions** inherit the value of `this` from the parent scope, we can re-write our function like this:

``` javascript
// grab our div with class box
const box = document.querySelector(".box");
// listen for a click event
box.addEventListener("click", function () {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(() => {
    // try to toggle again the class
    this.classList.toggle("open");
   });
});
```

Here, the second `this` will inherit from its parent, and will be set to the `const` box.

&nbsp;

## When you should avoid arrow functions

Using what we know about the inheritance of the `this` keyword we can define some instances where you should **not** use arrow functions.

The next 2 examples all show when to be careful using `this` inside of arrows.

``` javascript
const button = document.querySelector("btn");
button.addEventListener("click", () => {
  // error: *this* refers to the `Window` Object
  this.classList.toggle("on");
})
```

``` javascript
const person = {
  age: 10,
  grow: () => {
    // error: *this* refers to the `Window` Object
    this.age++;
  }
}
```

One other difference between Arrow functions and normal functions is access to the `arguments`. 

```javascript
const orderRunners = () => {
  const runners = Array.from(arguments);
  return runners.map((runner, i) => {
    return `#{runner} was number #{i +1}`;
   })
}
```

This code will return:

``` javascript
ReferenceError: arguments is not defined
```

To access all the arguments of an array, use old function notation, or the splat syntax. The name of the parameter does not matter (here it is called args to reduce confusion with `arguments`). 

```javascript
const orderRunners = (...args) => {
  const runners = Array.from(args);
  console.log(runners);
  return runners.map((runner, i) => {
    return `#{runner} was number #{i +1}`;
  })
}
```
