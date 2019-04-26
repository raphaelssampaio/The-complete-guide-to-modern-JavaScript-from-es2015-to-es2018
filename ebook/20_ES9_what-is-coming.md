# Capítulo 20: ES2018 o que está vindo?

ES 2018 ainda não foi lançado mas nós podemos ver as propostas das funcionalidades que chegaram ao estágio 4 (o estágio final) e que serão incluídas na nova versão que está vindo no ECMAScript.
Você pode encontrar a lista no [github](https://github.com/tc39/proposals/blob/master/finished-proposals.md).

&nbsp;

## Rest / Spread para Objetos
 
Agora nós podemos usar a sintaxe rest/spread para objetos, vamos ver como:

```js
let meuObj = { 
  a:1,
  b:3,
  c:5,
  d:8,
}

// nós usamos o operador rest para pegar tudo que sobrar do objeto.
let { a, b, ...z } = meuObj;
console.log(a);      // 1
console.log(b);      // 3
console.log(z);   // {c: 5, d: 8}

// usando a sintaxe spread nós clonamos nosso Objeto
let clone = { ...meuObj };
console.log(clone);
// {a: 1, b: 3, c: 5, d: 8}
```

&nbsp;

## Iteração Assíncrona

Com Iteração Assíncrona nós podemos iterar assincronamente nossos dados.

[Da documentação:](https://github.com/tc39/proposal-async-iteration)

[From the documentation:](https://github.com/tc39/proposal-async-iteration)
> Um iterador assíncrono é muito parecido com um iterador, exceto que seu método `next()` retorna uma promessa para um par `{ value, done }`.

Para fazer isso, nós usaremos uma estrutura de repetição `for-await-of`.

``` js
for await (const linha of lerLinhas(caminhoDoArquivo)) {
  console.log(linha);
}
```

> Durante a execução, um iterador assíncrono é criado a partir do dado fonte usando o método `[Symbol.asyncIterator]()`.
Cada vez que acessarmos o próximo valor na sequência, nós implicitamente esperamos a promessa retornada do método iterador

&nbsp;

## `Promise.prototype.finally()`

Depois da nossa promessa ter terminado nós podemos chamar um callback.

``` js
fetch("sua-url")
  .then(resultado => {
    // faça alguma coisa com o resultado
  })
  .catch(error => {
    // faça alguma coisa com o erro
  })
  .finally(()=> {
    // faça alguma coisa depois que a promessa for finalizada
  })
```

&nbsp;

## Funcionalidades RegExp

4 novos recursos relacionados ao RegExp chegarão à nova versão do ECMAScript. Eles são:

- [`s (dotAll)` flag para expressão regular](https://github.com/tc39/proposal-regexp-dotall-flag)
- [RegExp grupos de captura nomeados](https://github.com/tc39/proposal-regexp-named-groups)
- [RegExp Asserções Lookbehind](https://github.com/tc39/proposal-regexp-lookbehind)
- [RegExp Escapes da propriedade Unicode](https://github.com/tc39/proposal-regexp-lookbehind)

&nbsp;

### `s (dotAll)` flag para expressão regular

Isso apresenta uma nova flag `s` para as expressões regulares do ECMAScript que fazem `.` corresponder a qualquer caractere, incluindo o terminador de linha.

``` js
/foo.bar/s.test('foo\nbar');
// → true
```

&nbsp;

### RegExp named capture groups

[Da documentação:](https://github.com/tc39/proposal-regexp-named-groups)

>Numbered capture groups allow one to refer to certain portions of a string that a regular expression matches. Each capture group is assigned a unique number and can be referenced using that number, but this can make a regular expression hard to grasp and refactor.</br> </br> For example, given` /(\d{4})-(\d{2})-(\d{2})/` that matches a date, one cannot be sure which group corresponds to the month and which one is the day without examining the surrounding code. Also, if one wants to swap the order of the month and the day, the group references should also be updated.</br> </br> A capture group can be given a name using the `(?<name>...)` syntax, for any identifier `name`. The regular expression for a date then can be written as `/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u`. Each name should be unique and follow the grammar for ECMAScript IdentifierName.</br> </br> Named groups can be accessed from properties of a `groups` property of the regular expression result. Numbered references to the groups are also created, just as for non-named groups. For example:

> 

``` js
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
let result = re.exec('2015-01-02');
// result.groups.year === '2015';
// result.groups.month === '01';
// result.groups.day === '02';

// result[0] === '2015-01-02';
// result[1] === '2015';
// result[2] === '01';
// result[3] === '02';

let {groups: {one, two}} = /^(?<one>.*):(?<two>.*)$/u.exec('foo:bar');
console.log(`one: ${one}, two: ${two}`);  // prints one: foo, two: bar
```
&nbsp; 

### RegExp Lookbehind Assertions

[From the documentation:](https://github.com/tc39/proposal-regexp-lookbehind)

> With lookbehind assertions, one can make sure that a pattern is or isn't preceded by another, e.g. matching a dollar amount without capturing the dollar sign. </br></br> Positive lookbehind assertions are denoted as `(?<=...)` and they ensure that the pattern contained within precedes the pattern following the assertion. For example, if one wants to match a dollar amount without capturing the dollar sign, `/(?<=\$)\d+(\.\d*)?/` can be used, matching `'$10.53'` and returning `'10.53'`. This, however, wouldn't match `€10.53`.</br></br> Negative lookbehind assertions are denoted as `(?<!...) `and, on the other hand, make sure that the pattern within doesn't precede the pattern following the assertion. For example, `/(?<!\$)\d+(?:\.\d*)/` wouldn't match `'$10.53'`, but would `'€10.53'`.

&nbsp; 

### RegExp Unicode Property Escapes

[From the documentation:](https://github.com/tc39/proposal-regexp-unicode-property-escapes)

This brings the addition of Unicode property escapes of the form `\p{…}` and` \P{…}`. Unicode property escapes are a new type of escape sequence available in regular expressions that have the `u` flag set. With this feature, we could write:

``` js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π');
// → true
```

&nbsp;

## Lifting template literals restriction

When using *tagged* template literals the restriction on escape sequences are removed.

You can read more [here.](https://tc39.github.io/proposal-template-literal-revision/#sec-template-literals)
