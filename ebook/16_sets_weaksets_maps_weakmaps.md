# Chapter 16: Sets, WeakSets, Maps e WeakMaps

## O que é `Set`?

Um `Set` é um objeto onde nós podemos guardar **valores únicos** de qualquer tipo.

```js
// criando nosso set
const familia = new Set();

// adicionando valores a ele
familia.add("Pai");
console.log(familia);
// Set [ "Pai" ]

familia.add("Mãe");
console.log(familia);
// Set [ "Pai", "Mãe" ]

familia.add("Filho");
console.log(familia);
// Set [ "Pai", "Mãe", "Filho" ]

familia.add("Dad");
console.log(familia);
// Set [ "Pai", "Mãe", "Filho" ]
```

Como você pode ver, no final nós tentanmos adicionar "Pai" de novo, mas o `Set` não foi alterado porque um `Set` só pode ter **valores únicos**

Vamos continuar usando o mesmo `Set` e ver qual método nós podemos usar nele.

``` js
familia.size;
// 3
familia.keys();
// SetIterator {"Pai", "Mãe", "Filho"}
familia.entries();
// SetIterator {"Pai", "Mãe", "Filho"}
familia.values();
// SetIterator {"Pai", "Mãe", "Filho"}
familia.delete("Pai");
// true
familia;
// Set [ "Mãe", "Filho" ]
familia.clear;
familia;
// Set []
```

Como você pode ver um `Set` tem uma propriedade `size` e nós podemos excluir (`delete`) um item ou usar o `clear` para excluir todos os itens dele.

Nós podemos notar que um `Set` não tem chaves, então quando nós chamamos `.keys()` nós obtemos o mesmo que de quando nós chamamos `.values()` ou `.entries()`.

&nbsp;

### Loop over a `Set`

We have two ways of iterating over a `Set`: using `.next()` or using a `for of` loop.

``` js
// using `.next()`
const iterator = family.values();
iterator.next();
// Object { value: "Dad", done: false }
iterator.next();
// Object { value: "Mom", done: false }


// using a `for of` loop
for(const person of family) {
  console.log(person);
}
// Dad
// Mom
// Son
```

&nbsp;

### Remove duplicates from an array

We can use a `Set` to remove duplicates from an Array since we know it can only hold unique values.

```js
const myArray = ["dad", "mom", "son", "dad", "mom", "daughter"];

const set = new Set(myArray);
console.log(set);
// Set [ "dad", "mom", "son", "daughter" ]
// transform the `Set` into an Array
const uniqueArray = Array.from(set);
console.log(uniqueArray);
// Array [ "dad", "mom", "son", "daughter" ]

// write the same but in a single line
const uniqueArray = Array.from(new Set(myArray));
// Array [ "dad", "mom", "son", "daughter" ]
```

As you can see the new array only contains the unique values from the original array.

&nbsp;

## What is a `WeakSet`?

A `WeakSet` is similar to a `Set` but it can **only** contain Objects.


``` js
let dad = {name: "Daddy", age: 50};
let mom = {name: "Mummy", age: 45};

const family = new WeakSet([dad,mom]);

for(const person of family){
  console.log(person);
}
// TypeError: family is not iterable
```

We created our new `WeakSet` but when we tried to use a `for of` loop it did not work, we can't iterate over a `WeakSet`.

Another big difference that we can see is by trying to use `.clear` on a `WeakSet`: nothing will happen because a `WeakSet` cleans itself up after we delete an element from it.

```js
dad = null;
family;
// WeakSet [ {…}, {…} ]

// wait a few seconds
family;
// WeakSet [ {…} ]
```

As you can see after a few seconds **dad** was removed and *garbage collected*. That happened because the reference to it was lost when we set the value to `null`.


&nbsp;

## What is a `Map`?

A `Map` is similar to a `Set`, but they have key and value pairs.

```js
const family = new Map();

family.set("Dad", 40);
family.set("Mom", 50);
family.set("Son", 20);

family;
// Map { Dad → 40, Mom → 50, Son → 20 }
family.size;
// 3

family.forEach((key,val) => console.log(val,key));
// Dad 40
// Mom 50
// Son 20

for(const [key,val] of family){
  console.log(key,val);
}
// Dad 40
// Mom 50
// Son 20
```

If you remember, we could iterate over a `Set` only with a `for of` loop, while we can iterate over a `Map` with both a `for of` and a `forEach` loop.

&nbsp;

## What is a `WeakMap`?

A `WeakMap` is a collection of key/value pairs and similarly to a `WeakSet`, even in a `WeakMap` the keys are *weakly* referenced, which means that when the reference is lost, the value will be removed from the `WeakMap` and *garbage collected*.

A `WeakMap` is **not** enumerable therefore we cannot loop over it.

```js
let dad = { name: "Daddy" };
let mom = { name: "Mommy" };

const myMap = new Map();
const myWeakMap = new WeakMap();

myMap.set(dad);
myWeakMap.set(mom);

dad = null;
mom = null;

myMap;
// Map(1) {{…}}
myWeakMap;
// WeakMap {}
```

As you can see *mom* was garbage collected after we set the its value to `null` whilst *dad* still remains inside our `Map`.
