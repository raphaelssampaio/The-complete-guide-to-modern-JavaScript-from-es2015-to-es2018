# Capítulo 17: Todas as notivades na ES2016 (ES7)

ES2016 apresentou somente duas novas funcionalidades :

- `Array.prototype.includes()`
- O operador exponencial

&nbsp;

## `Array.prototype.includes()`

O método `includes()` retornará `true` se nosso array incluir um certo elemento, ou `false` se ele não incluir.

```js
let array = [1,2,4,5];

array.includes(2);
// true
array.includes(3);
// false
```

&nbsp;

### Combinar `inclues()` com `fromIndex`

Nós podemos fornecer `.includes()` com um index onde ele começa a procurar pelo elemento. O padrão é 0, mas nós podemos passar valores negativos também.

O primeiro valor que nós passamos é o elemento para procurar e o segundo o segundo é o index:

``` js
let array = [1,3,5,7,9,11];

array.includes(3,1);
// true
array.includes(5,4);
//false
array.includes(1,-1);
// false
array.includes(11,-3);
// true
```

`array.includes(5,4);` retornou `false` pois, apesar do array conter o número 5, ele foi encontrado no index 2 mas nós começamos a procurar na posição 4. Esse foi o motivo de nós não podermos encontrá-lo e o que foi retornado foi `false`.

`array.includes(1,-1);` retornou `false` porque nós começamos a proculá-lo no index -1 (que é o último elemento do array) e então continuou daquele ponto em diante.

`array.includes(11,-3);` retornou `true` porque nós voltamos ao index -3 e subiu, encontrando o valor 11 no nosso caminho.

&nbsp;

## O operador exponencial

Antes do ES2016 nós poderíamos fazer assim:

``` js
Math.pow(2,2);
// 4
Math.pow(2,3);
// 8
```

Agora com o novo operador exponencial nós podemos fazer assim:

```js
2**2;
// 4
2**3;
// 8
```

Ele será muito útil quando combinado com operadores múltiplos como no exemplo abaixo:

``` js
2**2**2;
// 16
Math.pow(Math.pow(2,2),2);
// 16
```

Usando `Math.pow()` você precisa continuamente concatená-los e eles vão ficando longos e bangunçados. O operador exponencial fornece uma rápida e limpa maneira de fazer a mesma coisa.
