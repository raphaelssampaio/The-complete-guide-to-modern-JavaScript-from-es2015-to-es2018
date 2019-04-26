# Capítulo 18: ES2017 padding de string, `Object.entries()`, `Object.values()` e mais

ES2017 apresentou várias novas funcionalidades legals, que nós vamos ver aqui.

## Padding de String(`padStart` e `padEnd`)

Nós podemos adicionar padding para nossas strings, tanto no final (`padEnd`) quanto no começo delas (`padStart`).

```js
"olá".padStart(6);
// " ola"
"olá".padEnd(6);
// "olá "
```

Nós especificamos que queremos 4 de padding, mas por que em ambos os casos nós obtivemos apenas 1 espaço?
Isso acontece porque `padStart` e `padEnd` vai preencher o **espaço vazio**. No nosso exemplo "olá" tem 3 letras, e nosso padding é de 4, o que deixa somente 1 espaço vazio.

Look at this example:

```js
"oi".padStart(10);
// 10 - 2 = 8 espaços vazios
// "        oi"
"bem-vindo".padStart(16);
// 16 - 9 = 7 espaço vazio
// "       bem-vindo"
```

&nbsp;

### Alinhamento a direita com `padStart`

Nós podemos usar `padStart` se nós queremos alinhar algo a direita.

```js
const strings = ["pequena", "tamanho médio", "string muito grande"];

const stringMaisLonga = strings.sort(str => str.length)
                              .map(str => str.length)[0];

strings.forEach(str => console.log(str.padStart(stringMaisLonga)));

// string muito grande
//       tamanho médio
//             pequena
```

Primeiro, nós pegamos a maior de nossas strings e mensuramos seu tamanho. Então nós aplicamos um `padStart` para todas as strings baseado no maior tamanho então nós agora temos todos perfeitamente alinhados a direita.

&nbsp;

### Adicionar um valor personalizado para o padding 

Nós não somos obrigados a adicionar um espaço em branco para o padding, nós podemos passar tanto string quanto números.

```js
"olá".padEnd(11," Alberto");
// "olá Alberto"
"1".padStart(3,0);
// "001"
"99".padStart(3,0);
// "099"
```

&nbsp;

## `Object.entries()` e `Object.values()`

Vamos primeiro criar um Objeto.

```js
const familia = {
  pai: "Jonathan Kent",
  mae: "Martha Kent",
  filho: "Clark Kent",
}
```

Em versões anteriores de JavaScript nós teríamos que acessar os valores dentro do objeto assim:

```js
Object.keys(familia);
// ["pai", "mae", "filho"]
familia.pai;
"Jonathan Kent"
```

`Object.keys()` nos retornou somente as chaves dos objetos que nós, então, usamos para acessar os valores.

Nós agora temos duas maneiras a mais de acessar nossos objetos:

```js
Object.values(familia);
// ["Jonathan Kent", "Martha Kent", "Clark Kent"]

Object.entries(familia);
// ["pai", "Jonathan Kent"]
// ["mae", "Martha Kent"]
// ["filho", "Clark Kent"]
```

`Object.values()`retorna um array de todos os valores enquanto `Object.entries()` retorna um array de arrays contendo tanto chaves (keys) quando valores.

&nbsp;

## `Object.getOwnPropertyDescriptors()`

This method will return all the own property descriptors of an object.
The attributes it can return are `value`, `writable`, `get`, `set`, `configurable` and `enumerable`.

Esse método vai retornar todas os descritores de propriedade próprios de um objeto.

``` js
const meuObj = {
  nome: "Alberto",
  idade: 25,
  saude() {
    console.log("olá");
  },
}
Object.getOwnPropertyDescriptors(meuObj);
// idade:{value: 25, writable: true, enumerable: true, configurable: true}

// saude:{value: ƒ, writable: true, enumerable: true, configurable: true}

// nome:{value: "Alberto", writable: true, enumerable: true, configurable: true}
```

&nbsp;

## Vírculas à direita em lista de parâmetros de funções e em chamadas

Isso é apenas uma mudança menor na sintaxe. Agora, quando escrevemos objetos nós precisamos deixar uma vírgula a direita depois de cada parâmetro, se for o último parâmetro ou não.

``` js
// era assim
const objeto = {
  prop1: "prop",
  prop2: "propop"
}

// agora é assim
const objeto = {
  prop1: "prop",
  prop2: "propop",
}
```

Note como eu escrevo uma vírgula no final da segunda propriedade.
Não irá disparar nenhum erro se você não colocar, mas é uma melhor prática para seguir pois facilitará a vida dos seus colegas ou membros da equipe.


```js
// Eu escrevo
const object = {
  prop1: "prop",
  prop2: "propop"
}


// meus colegas atualizam o código, adicionando uma nova propriedade
const object = {
  prop1: "prop",
  prop2: "propop"
  prop3: "propopop"
}
// de repente, ele recebe um erro porque ele não notou que eu esqueci de colocar uma vírgula no final do último parâmetro
```


&nbsp;

## Memória compartilhada e `Atomics`

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics):

> Quando a memória é compartilhada, multiplas threads podem ser lidas e escritas no mesmo dado da memória. Operações atômicas garantem que os valores previstos sejam lidos e escritos, estas operações são finalizadas antes da próxima operação iniciar e que as mesmas não sejam interrompidas.

`Atomics` is not a constructor, all of its properties and methods are static (just like `Math`) therefore we cannot use it with a new operator or invoke the `Atomics` object as a function.

`Atomics` não é um construtor, todas as suas propriedades e métodos são estáticos (do mesmo jeito de `Math`) portanto, nós não podemos usá-lo como um novo operador ou chamar o objeto `Atomics` como uma função.


Examples of its methods are:

Alguns exemplos dos seus métodos são:

- add / sub
- and / or / xor
- load / store
