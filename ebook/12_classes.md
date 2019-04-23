# Capítulo 12: Classes

Citando o MDN:
> classes são simplificações da linguagem para as heranças baseadas nos protótipos. A sintaxe para classes **não** introduz um novo modelo de herança de orientação a objetos em JavaScript.

Dito isso, vamos rever a herança prototipal antes de pularmos para as classes.

``` js
function Pessoa(nome, idade) {
  this.nome = nome;
  this.idade = idade;
}

Pessoa.prototype.saude = function(){
  console.log("Olá, meu nome é " + this.nome);
}

const alberto = new Pessoa("Alberto", 25);
const caroline = new Pessoa("Caroline",25);

alberto.saude();
// Olá, meu nome é Alberto
caroline.saude();
// Olá, meu nome é Caroline
```

Nós adicionamos um novo método do protótipo a fim de tornar acessível para todos as novas instâncias de Pessoa que nós criamos.

Ok, agora que eu refresquei seu conhecimento de herança prototipal, vamos olhar as classes.

&nbsp;

## Crie uma `Classe`

Existem duas maneiras de criar uma classe:

- declaração de classe
- expressão de classe

``` js
// declaração de classe
class Pessoa {

}

// expressão de classe
const pessoa = class Pessoa {
}
```

>Lembre-se: declaração de classe (e expressão) **não são elevados**, o que significa que a menos que você queira obter um **ReferenceError** você precisa declarar sua classe antes de acessá-la.

Vamos começar criando nossa primeira `Classe`.

Nós só precisamos de um método chamado `constructor`(lembre-se de adicionar somente um construtor, um `SyntaxError` aparacerá se a classe tiver mais de um método construtor).

``` js
class Pessoa {
  constructor(nome, idade){
    this.nome = nome;
    this.idade = idade;
  }
  saude(){
    console.log(`Olá, meu nome é ${this.nome} e eu tenho ${this.idade} de idade` );
  } // sem comandos entre os métodos
  despedida(){
    console.log("adeus amigo");
  }
}

const alberto = new Pessoa("Alberto",25);

alberto.saude();
// Olá, meu nome é Alberto e eu tenho 25 anos de idade
alberto.despedida();
// adeus amigo
```

Como você pode ver, tudo trabalha como antes. Como nós mencionamos no começo, Classes são simplificações da linguagem, um jeito melhor de fazer herança.

&nbsp;

## Métodos Estáticos

Agora os dois métodos que nós criamos, `saude()` e `adeus()` podem ser acessados por todas as novas instâncias de `Pessoa`, mas o e se nós quisermos um método que só pode ser acessado pela própria classe, similarmente ao `Array.of()` para arrays?

```js
static info(){
  console.log("Eu sou uma classe Pessoa, prazer em conhecê-lo");
}

alberto.info();
// TypeError: alberto.info não é uma função

Pessoa.info();
// Eu sou uma classe Pessoa, prazer em conhecê-lo
```

&nbsp;

## `set` e `get`

Nós usamos métodos setters e getters para definir valores e recuperar valores dentro da nossa `Class` respectivamente.

```js
class Pessoa {
  constructor(nome, sobrenome) {
    this.nome = nome;
    this.sobrenome = sobrenome;
    this.apelido = "";
  }
  set apelidos(valor){
    this.apelido = valor;
  }
  get apelidos(){
    return `Seu apelido é ${this.apelido}`;
  }
}

const alberto = new Pessoa("Alberto","Montalesi");

// primeiro nós chamamos o setter
alberto.apelidos = "Albi";
// "Albi"

// então nós chamamos o getter
alberto.apelidos;
// "Seu apelido é Albi"
```

&nbsp;

## Expandindo nossa `Classe`

E se nós quisermos ter uma nova `Classe` que herda da nossa classe anterior? Nós usamos `extends`:

``` js
// nossa classe inicial
class Pessoa {
  constructor(nome, idade){
    this.nome = nome;
    this.idade = idade;
  }
  saude(){
    console.log(`Olá, meu nome é ${this.nome} e eu tenho ${this.idade} de idade` );
  }
}


// nossa nova classe
class Adulto extends Pessoa {
  constructor(nome, idade, trabalho){
    this.nome = nome;
    this.idade = idade;
    this.trabalho = trabalho;
  }
}

const alberto = new Adulto("Alberto",25,"professor");
```

Nós criamos uma nova `Classe Adulto` que herda de `Pessoa` mas se você tentar rodar esse código você vai obter um erro:

```js
ReferenceError: deve chamar um super construtor antes de usar |this| no construtor da classe Adulto
```

A mensagem de erro nos fala para chamarmos `super()` antes de usar `this` na nossa nova `Classe`.
O que isso significa é que basicamente nós temos que criar uma nova Pessoa antes de criar um novo Adulto e o `super()` no construtor vai fazer exatamente isso.

``` js
class Adulto extends Pessoa {
  constructor(nome, idade, trabalho){
    super(nome, idade);
    this.trabalho = trabalho;
  }
}
```

Por que nós definimos `super(nome, idade)` ? Porque nossa classe `Adulto` herda um nome e uma idade de `Pessoa`, portanto nós não precisamos declará-los.
Super vai simplesmente criar uma nova Pessoa para nós.

Se nós agora rodarmos o código novamente vamos obter o seguinte:

``` js
alberto.idade
// 25
alberto.trabalho
// "professor"
alberto.saude();
// Olá, meu nome é Alberto e eu tenho 25 anos de idade
```

Como você pode ver, nosso `Adulto` herda todas as propriedades e métodos da classe `Pessoa`.

&nbsp;

## Extendendo Arrays

Nós queremos criar algo como isso, alguma coisa similar a um array onde o primeiro valor é uma propriedade que define nossa Sala de Aula e o resto dos nossos estudantes e suas notas.

``` js
// nós criamos uma nova Sala de Aula
const minhaTurma = new SalaDeAula('1A', 
  {nome: "Tim", nota: 6},
  {nome: "Tom", nota: 3},
  {nome: "Jim", nota: 8},
  {nome: "Jon", nota: 10},
);
```

O que nós podemos fazer é criar uma nova `Classe` que extender o array.

``` js
class SalaDeAula extends Array {
  // nós usamos o operador rest para pegar todos os estudantes
  constructor(nome, ...estudantes){
    // nós usamos o spread para colocar todos os estudantes em um array individualmente, de outra forma nós colocamos um array para dentro de outro array
    super(...estudantes);
    this.nome = nome;
    // nós criamos um novo método para adicionar estudantes
  } add(estudante){
    this.push(estudante);
  }
}
const minhaTurma = new SalaDeAula('1A', 
  {nome: "Tim", nota: 6},
  {nome: "Tom", nota: 3},
  {nome: "Jim", nota: 8},
  {nome: "Jon", nota: 10},
);

// agora nós chamamos
minhaTurma.add({nome: "Timmy", nota:7});
minhaTurma[4];
// Object { nome: "Timmy", nota: 7 }

// nós podemos também usar o for of
for(const estudante of minhaTurma) {
  console.log(estudente);
  }
// Object { nome: "Tim", nota: 6 }
// Object { nome: "Tom", nota: 3 }
// Object { nome: "Jim", nota: 8 }
// Object { nome: "Jon", nota: 10 }
// Object { nome: "Timmy", nota: 7 }
```
