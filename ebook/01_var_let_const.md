# Capítulo 1: Var vs Let vs Const e a zona morta temporal

Com a introdução de `let` e `const` no **ES6**, nós podemos definir melhor das nossas variáveis dependendo do que nós queremos. Vamos ver as maiores diferenças entre elas.

&nbsp;

## `Var`

`var` são **funções escopo**, que significa que se nós as declaramos dentro de um `for` (que é um escopo **fechado**) elas estarão disponíveis fora deste escopo.

``` javascript 
for (var i = 0; i < 10; i++) {
  var leak = "Eu estou disponível fora do loop";
}

console.log(leak);
// Eu estou disponível fora do loop

function myFunc(){
  var functionScoped = "Eu estou disponível dentro desta função";
  console.log(functionScoped);
}
myFunc();
// Eu estou disponível dentro desta função
console.log(functionScoped);
// ReferenceError: functionScoped is not defined
```

No primeiro exemplo o valor de `var` vazou para fora do escopo-fechado e pode ser acessada fora dele, enquanto no segundo  exemplo `var` foi confinada dentro de uma função-escopo e nós não podemos acessá-la fora dele.

&nbsp;

## `Let`

`let` (e `const`) são **escopo fechado**, significando que elas estão disponíveis somente dentro de um bloco onde elas são declaradas e em seus sub-blocos.

``` javascript
// usando `let`
let x = "global";

if (x === "global") {
  let x = "escopo-fechado";

  console.log(x);
  // output esperado: escopo-fechado
}

console.log(x);
// output esperado: global

// usando `var`
var y = "global";

if (y === "global") {
  var y = "escopo-fechado";

  console.log(y);
  // output esperado: escopo-fechado
}

console.log(y);
// output esperado: escopo-fechado
```

Como nós podemos ver, quand nós assinamos um novo valor ao nosso `let` dentro do nosso escopo-fechado, ele **não** muda seu valor no escopo global, enquanto quando nós fazemos a mesma coisa com nosso `var` ele vaza para fora do nosso escopo-fechado e também muda o valor no escopo global.

&nbsp;

## `Const`

Do mesmo modo de `let`, `const` são **escopo-fechado**, mas eles diferem no fato do valor deles **não mudarem através da reassinatura ou não poderem ser redeclarados**.

``` javascript
const constant = 'Eu sou uma constante';
constant = " Eu não posso ser reassinada";

// Uncaught TypeError: Atribuição a uma variável constante
```


**Importante** 
Isso **não** significa que **const são imutáveis**.

&nbsp;

### O conteúdo de uma `const` é um Objeto

``` javascript
const person = {
  name: 'Alberto',
  age: 25,
}

person.age = 26;
console.log(person.age);
// 26
// nesse caso nenhum erro será mostrado, nós não estamos reassinando a variável mas somente uma de suas propriedades.
```

---

&nbsp;

## A zona morta temporal

De acordo com **MDN**:

> No ECMAScript 2015, as let não estão sujeitas a **Variável de Hoisting**, o que significa que as declarações let não se movem para o topo do contexto de execução atual. Fazer referência à variável no bloco antes da inicialização resulta em um ReferenceError (ao contrário de uma variável declarada com var, que terá apenas o valor indefinido). A variável está em uma “zona morta temporal” desde o início do bloco até que a inicialização seja processada.

Let's look at an example:

```javascript
console.log(i);
var i = "Eu sou uma variável";

// output esperado: undefined

console.log(j);
let j = "Eu sou um let";

// output esperado: ReferenceError: não é possível acessar a declaração lexical `j' antes da inicialização
```

`var` podem ser acessadas **antes** delas serem definidas, mas nós não conseguimos acessar o **valor** delas.

`let` e `const` não podem ser acessadas **antes de nós as definirmos**.

Isso acontece porque `var` são objetos de **hoisting**, que significa que elas são processadas antes de qualquer código ser executado. Declarar uma `var` em qualquer lugar é equivalente a **declará-la no topo**. Este é o motivo de nós ainda assim conseguirmos acessar a `var` mas nós ainda não conseguimos ver seu conteúdo, por isso o resultado é `undefined`.


---
&nbsp;

## Quando usar `Var`, `Let` e `Const`

Não há regra afirmando onde usar cada uma delas e as pessoas tem opiniões diferentes. Aqui eu vou apresentar a você duas opiniões de desenvolvedores conhecidos na comunidade JavaScript.

A primeira opinião vem de [Mathias Bynes:](https://mathiasbynens.be/notes/es6-const)


- use `const` como padrão
- use `let` somente se for necessário mudar o valor.
- `var` nunca devem ser usadas no ES6.


A segunda opinião vem de [Kyle Simpson:]( blog.getify.com/constantly-confusing-const/)

- Use `var` para as variáveis top-level que são compartilhadas por muitos (e especialmente grandes) escopos.
- Use `let` para variáveis locais em pequenos escopos.
- Refatorar `let` para `const` somente antes de algum código que foi escrito, e você está razoavelmente certo que você tem um caso onde não será necessário fazer uma reassinatura.

Which opinion to follow is entirely up to you. As always, do your own research and figure out which one you think is the best.
A escolha de qual opinião seguir é sua. Como sempre, faça sua própria pesquisa e descubra qual você acha ser a melhor definição.
