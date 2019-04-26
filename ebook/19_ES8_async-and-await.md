# Capítulo 19: ES2017 Async e Await

ES2017 apresentou uma nova maneira de trabalhar com promessas chamada "async/await".

&nbsp;

## Revisão de `Promesa`

Antes de nós mergulharmos na nova sintaxe, vamos rapidamente revisar como nós usualmente escreveríamos uma promessa:

```js
// buscar (fetch) um usuário do github
fetch('api.github.com/user/AlbertoMontalesi').then( res => {
  // returna a data no formato json
  return res.json();
}).then(res => {
  // se tudo correr bem, printa a data
  console.log(res);
}).catch( err => {
  // ou printa o erro
  console.log(err);
})
```

Essa é uma promessa bem simples para buscar um usuário do GitHub e printar ele no console.

Vamos ver um exemplo diferente:

```js
function andar(valor) {
  return new Promise((resolve,reject) => {
    if (valor < 500) {
      reject ("o valor é muito pequeno");
    }
    setTimeout(() => resolve(`você andou por ${valor}ms`),valor);
  });
}

andar(1000).then(res => {
  console.log(res);
  return andar(500);
}).then(res => {
  console.log(res);
  return andar(700);
}).then(res => {
  console.log(res);
  return andar(800);
}).then(res => {
  console.log(res);
  return andar(100);
}).then(res => {
  console.log(res);
  return andar(400);
}).then(res => {
  console.log(res);
  return andar(600);
});

// você andou por 1000ms
// você andou por 500ms 
// você andou por 700ms 
// você andou por 800ms 
// uncaught exception: o valor é muito pequeno
```

Vamos ver como nós podemos reescrever essa `Promessa` com a nova sintaxe async/await.

&nbsp;

## Async e Await

``` js
function andar(valor) {
  return new Promise((resolve,reject) => {
    if (valor < 500) {
      reject ("o valor é muito pequeno");
    }    
    setTimeout(() => resolve(`você andou por ${valor}ms`),valor);
  });
}

// create an async function
async function ir() {
  // use a palavra-chave `await` para esperar a resposta
  const res = await andar(500);
  console.log(res);
  const res2 = await andar(900);
  console.log(res2);
  const res3 = await andar(600);
  console.log(res3);
  const res4 = await andar(700);
  console.log(res4);
  const res5 = await andar(400);
  console.log(res5);
  console.log("finalizado");
}

ir();



// você andou por 500ms 
// você andou por 900ms
// você andou por 600ms 
// você andou por 700ms 
// uncaught exception: o valor é muito pequeno
```

Vamos destrinchar o que nós acabamos de fazer:

- para criar uma função `assíncrona` nós precisamos colocar a palavra chave `async` na frente dela
- a palavra chave irá dizer ao JavaScript para sempre retornar uma promessa
- se nós especificarmos para `retorn <non-promise>` ele vai retornar um valor envolto dentro de uma promessa
- a palavra-chave `await` **só** trabalha dentro de uma função `assíncrona`.
- como o nome inplica, `await` diz ao JavaScript para esperar até a promessa retornar seu valor

Vamos ver o que acontece se nós tentarmos usar `await` fora de uma função `assíncrona`

```js
// usando await dentro de uma função normal
function func() {
  let promessa = Promise.resolve(1);
  let resultado = await promessa; 
}
func();
// SyntaxError: await is only valid in async functions and async generators
// Tradução: SyntaxError: await só é válido dentro de funções assíncronas e geradores assíncronos



// usando await no código normal
let resposta = Promise.resolve("hi");
let resultado = await resposta;
// SyntaxError: await is only valid in async functions and async generators
// Tradução: SyntaxError: await só é válido dentro de funções assíncronas e geradores assíncronos
```

> **Lembre-se** que você só pode usar `await` dentro de uma função `assíncrona`.

&nbsp;

## Tratando erros

Em uma promessa normal nós usaríamos `.catch()` para tratar eventuais erros retornados pelas promessas.
Aqui, nós não é muito diferente:

```js
async function funcaoAssincrona() {

  try {
    let resposta = await fetch('http:sua-url');
    } catch(err) {
        console.log(err);
      }
}

funcaoAssincrona();
// TypeError: failed to fetch
```

Quando usamos `try...catch` para pegar o erro, mas em um caso em que não os temos ainda podemos pegar o erro assim:

``` js
async function funcaoAssincrona(){
  let resposta = await fetch('http:sua-url');
}
funcaoAssincrona();
// Uncaught (in promise) TypeError: Failed to fetch
// Tradução: Não tratado (em promessa) TypeError: Falha ao buscar

funcaoAssincrona().catch(console.log);
// TypeError: Failed to fetch
// Tradução: Falha ao buscar
```
