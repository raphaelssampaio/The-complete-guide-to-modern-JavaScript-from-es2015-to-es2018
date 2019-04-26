# Capítulo 9: Operador spread e parâmetros res

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

### Copiar arrays

A sintaxe spread é muito útil se nós queremos criar uma cópia de um array.

``` js
const vegetais = ["tomate", "pepino", "feijão"];
const novosVegetais = vegetais;

// isso pode parecer que nós criamos uma cópia de um array vegetal mas veja agora

vegetais.push("ervilhas");
console.log(vegetais);
// Array [ "tomate", "pepino", "feijão", "ervilhas" ]

console.log(novosVegetais);
// Array [ "tomate", "pepino", "feijão", "ervilhas" ]
```

Nosso novo array também mudou, mas por quê? Porque na verdade nós não criamos uma cópia mas simplesmente referenciamos nosso antigo array em um novo.
É assim como nós normalmente fazemos uma cópia de um array no ES5 e antes dele.

``` js
const vegetais = ["tomate", "pepino", "feijão"];
const novosVegetais = [].concat(vegetais);
// nós criamos um array vazio e colocamos os valores do antigo array dentro dele
```

E é assim como nós faríamos usando a sintaxe spread:

``` js
const vegetais = ["tomate", "pepino", "feijão"];
const novosVegetais = [...vegetais];
```

&nbsp;

### Spread dentro de uma função

``` js
// Jeito antigo
function facaAlgo (x, y, z) {
  console.log(x + y + z);
 }
var args = [0, 1, 2];

// Chama a função, passando args
facaAlgo.apply(null, args);

// USE A SINTAXE SPREAD
facaAlgo(...args);
// 3 (0+1+2);
console.log(args);
// Array [ 0, 1, 2 ]
```



Nós podemos trocar a sintaxe `.apply()` e só usar o operador spread.

Vamos ver outro exemplo:

``` js
const nome = ["Alberto", "Montalesi"];

function saude(primeiro, ultimo) {
  console.log(`Olá ${primeiro} ${ultimo}`);
}

saude(...nome);
// Olá Alberto Montalesi
```

Os dois valores do array são automaticamente atribuídos para os dois argumentos da nossa função.

E se nós fornecermos mais valores do que argumentos?

``` js
const nome = ["Jon", "Paul", "Jones"];

function saude(primeiro, ultimo) {
  console.log(`Olá ${primeiro} ${ultimo}`);
}
saude(...nome);
// Olá Jon Paul
```

Nós fornecemos 3 valores dentro do nosso array mas só temos 2 argumentos na nossa função, portanto o último é deixado de lado.

&nbsp;

### Spread em Objetos Literais (ES 2018 / ES9)

Esta funcionalidade não é parte da ES6, mas como nós já estamos discutindo este tópico, vale a pena mencionar que no ES2018 irá apresentar o operador Spread para Objetos.
Vejamos o exemplo:

``` js
let pessoa = {
  nome : "Alberto",
  sobrenome: "Montalesi",
  idade: 25,
}

let clone = {...pessoa};
console.log(clone);
// Object { nome: "Alberto", sobrenome: "Montalesi", idade: 25 }
```

Nós podemos ler mais sobre o ES2018 no Capítulo 20.

---
&nbsp;

## O parâmetro Rest

A sintaxe rest parece exatamente a mesma coisa do spread, 3 pontos `...` mas é justamente o oposto. O spread expande um array, enquanto o rest condensa múltiplos elementos em um único.

```js

const corredores = ["Tom", "Paul", "Mark", "Luke"];
const [primeiro, segundo, ...perdedores] = corredores;

console.log(...perdedores);
// Mark Luke
```

Nós armazenamos o primeiro e o segundo valor dentro da `const` primeiro e segundo e resto foi colocado dentro de perdedores usando o parâmetro rest.
