# Capítulo 5: Outros métodos de strin

## Outros métodos de string

Nós iremos falar sobre 4 novos métodos de string:

- `startsWith()`
- `endsWith()`
- `includes()`
- `repeat()`

&nbsp;

### `startsWith()`

Este novo método irá checar se a string começa com o valor que nós passamos:

```js
const code = "ABCDEFG";

code.startsWith("ABB");
// false
code.startsWith("abc");
// false, startsWith é case sensitive
code.startsWith("ABC");
// true
```

Nós passamos um parâmetro adicional, que é o ponto de partida onde o método começará a checar.


``` js
const code = "ABCDEFGHI"

code.startsWith("DEF",3);
// true, ele começará a checar depois de 3 caractéres
```

&nbsp;

### `endsWith()`

Do mesmo modo de `startsWith()` este novo método checará se a string termina com o valor que nós passamos:

```js
const code = "ABCDEF";

code.endsWith("DDD");
// false
code.endsWith("def");
// false, endsWith é case sensitive
code.endsWith("DEF");
// true

```

Nós passamos um parâmetro adicional, que é on número de dígitos que nós queremos considerar quando checar o final.

``` js
const code = "ABCDEFGHI"

code.endsWith("EF", 6);
// true, 6 significa que nós consideramos apenas os 6 primeiros valores ABCDEF, e sim, esta string termina com EF, portanto nós temos *true*
```

&nbsp;

### `includes()`

Este método checará se nossa string inclui o valor que nós passamos.

```js
const code = "ABCDEF"

code.includes("ABB");
// false
code.includes("abc");
// false, includes é case sensitive
code.includes("CDE");
// true
```

&nbsp;

### `repeat()`

O nome é sugestivo, este novo método repetirá o que nós passamos.

``` js
let ola = "Oi";
console.log(ola.repeat(10));
// "OiOiOiOiOiOiOiOiOiOi"
```
