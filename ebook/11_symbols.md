# Capítulo 11: Symbols

ES6 adicionou um novo tipo primitivo chamado **Symbols**. O que eles são? E o que eles fazem?

Symbols são **sempre únicos** e nós podemos usá-los como identificadores para propriedades de objetos.

Vamos criar um `Symbol` juntos:

``` js
const eu = Symbol("Alberto");
console.log(eu);
// Symbol(Alberto)
```

Nós dizemos que eles são sempre únicos, vamos tentar criar um novo symbol com o mesmo valor e ver o que acontece:

``` js
const eu = Symbol("Alberto");
console.log(eu);
// Symbol(Alberto)

const clone = Symbol("Alberto");
console.log(clone);
// Symbol(Alberto)

console.log(eu == clone);
// false
console.log(eu === clone);
// false
```

Ambos tem o mesmo valor, mas nós nunca iremos ter colisões de nomes com Symbols porque eles são únicos.

Como mencionamos anteriormente, nós podemos usá-los para criar identificadores para propriedades de objetos, vamos ver um exemplo:

``` js 
const escritorio = {
  "Tom" : "CEO",
  "Mark": "CTO",
  "Mark": "CIO",
}

for (pessoa in escritorio){
  console.log(pessoa);
}
// Tom
// Mark
```

Aqui nós temos nosso objeto escritorio com 3 pessoas, duas delas tem o mesmo nome.
Para evitar colisões de nomes nós podemos usar symbols.

``` js
const escritorio = {
  [Symbol("Tom")] : "CEO",
  [Symbol("Mark")] : "CTO",
  [Symbol("Mark")] : "CIO",
}

for(pessoa in escritorio) {
  console.log(pessoa);
}
// undefined
```

Nós obtemos undefined quando nós tentamos usar estruturas de repetições por cima dos symbols pois eles **não são enumeráveis** então nós não podemos usar o `for in` neles. 

Se nós quisermos recuperar as propriedades de objetos nós podemos usar `Object.getOwnPropertySymbols()`.

``` js
const escritorio = {
  [Symbol("Tom")] : "CEO",
  [Symbol("Mark")] : "CTO",
  [Symbol("Mark")] : "CIO",
};

const symbols = Object.getOwnPropertySymbols(escritorio);
console.log(symbols);
// 0: Symbol(Tom)
​// 1: Symbol(Mark)
​// 2: Symbol(Mark)
​// length: 3
```

Nós recuperamos o array mas para ser capaz de acessar as propriedades nós usamos `map`.

```js
const symbols = Object.getOwnPropertySymbols(escritorio);
const valor = symbols.map(symbol => office[symbol]);
console.log(valor);
// 0: "CEO"
​// 1: "CTO"
​// 2: "CIO"
​// length: 3
```

Finalmente nós agora obtivemos o array contendo todos os valores dos nossos symbols.
