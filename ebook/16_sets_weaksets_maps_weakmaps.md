# Capítulo 16: Sets, WeakSets, Maps e WeakMaps

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

### Estruturas de repetição sobre um `Set`

Nós temos duas maneiras de iterar sobre um `Set`: usando `.next()` ou usando a estrutura `for of`.

``` js
// usando `.next()`
const iterador = familia.values();
iterador.next();
// Object { value: "Pai", done: false }
iterador.next();
// Object { value: "Mãe", done: false }


// usando um `for of`
for(const pessoa of familia) {
  console.log(pessoa);
}
// Pai
// Mãe
// Filho
```

&nbsp;

### Remover duplicatas de um array

Nós podemos usar um `Set` para remover duplicatas de um Array já que sabemos que só pode conter valores únicos.

```js
const meuArray = ["pai", "mãe", "filho", "pai", "mãe", "filha"];

const set = new Set(meuArray);
console.log(set);
// Set [ "pai", "mãe", "filho", "filha" ]
// transformando o `Set` em um Array
const arrayUnico = Array.from(set);
console.log(arrayUnico);
// Array [ "pai", "mãe", "filho", "filha" ]

// escrevendo a mesma coisa mas em uma única linha
const arrayUnico = Array.from(new Set(meuArray));
// Array [ "pai", "mãe", "filho", "filha" ]
```

Como você pode ver o novo array só contém os valores únicos do array original.

&nbsp;

## O que é um `WeakSet`?

Um `WeakSet` é similar ao `Set`mas ele **só** pode conter Objetos.


``` js
let pai = {nome: "Papai", idade: 50};
let mae = {nome: "Mamãe", idade: 45};

const familia = new WeakSet([pai,mae]);

for(const pessoa of familia){
  console.log(pessoa);
}
// TypeError: familia is not iterable  (família não é iterável)
```

Nós criamos um `WeakSet` mas quando nós tentamos usar uma estrutura `for of`, ela não funciona, nós não podemos iterar sobre um `WeakSet`.

Uma outra grande diferença que nós podemos ver é quando tentamos usar `.clear` em um `WeakSet`: nada acontecerá porque um `WeakSet` limpa a si mesmo depois que nós excluimos um elemento dele.

```js
pai = null;
familia;
// WeakSet [ {…}, {…} ]

// espere alguns segundos
familia;
// WeakSet [ {…} ]
```

Como você pode ver depois de algus segundos **pai** foi removido e o *lixo foi coletado*. Isso acontece porque a referência dela foi perdida quando nós setamos o valor para `null`.

&nbsp;

## O que é um `Map`?

Um `Map` é similar ao `Set`, mas eles têm chave e pares de valor.

```js
const familia = new Map();

familia.set("Pai", 40);
familia.set("Mãe", 50);
familia.set("Filho", 20);

familia;
// Map { Pai → 40, Mãe → 50, Filho → 20 }
familia.size;
// 3

familia.forEach((key,val) => console.log(val,key));
// Pai 40
// Mãe 50
// Filho 20

for(const [key,val] of familia){
  console.log(key,val);
}
// Pai 40
// Mãe 50
// Filho 20
```

Se você lembrar, nós podemos iterar sobre um `Set` somente com uma estrutura `for of`, enquanto no `Map` nós podemos iterar usando ambas as estruturas `for of` e `forEach`.

&nbsp;

## O que é um `WeakMap`?

Um `WeakMap` é uma coleção de chave/pares de valor e é similar ao `WeakSet`, mesmo em um `WeakMap` as chaves são referenciadas *fragilmente*, que significa que quando a referencia é perdida, o valor será removido do `WeakMap`e o *lixo coletado*.


Um `WeakMap` **não** é enumerável, portanto nós não conseguimos usar estruturas de repetição nele.

```js
let pai = { nome: "Papai" };
let mae = { nome: "Mamãe" };

const meuMap = new Map();
const meuWeakMap = new WeakMap();

meuMap.set(dad);
meuWeakMap.set(mom);

pai = null;
mae = null;

meuMap;
// Map(1) {{…}}
meuWeakMap;
// WeakMap {}
```

Como você pode ver *mae* foi coletado depois de nós setarmos o seu valor para `null` enquanto *pai* ainda continuou dentro do nosso `Map`.
