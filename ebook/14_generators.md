# Capítulo 14: Geradores

## O que é um Gerador?

Um função gedador é uma função que nós podemos startar ou parar, por tempo indefinido, e restartar com a possibilidade de passar dados adicionais em um ponto posterior no tempo.

Para criar uma função gerador nós fazemos assim:

``` js
function* frutaLista(){
  yield 'Banana';
  yield 'Maçã';
  yield 'Laranja';
}

const frutas = frutaLista();

frutas;
// Gerador
frutas.next();
// Object { value: "Banana", done: false }
frutas.next();
// Object { value: "Maçã", done: false }
frutas.next();
// Object { value: "Laranja", done: false }
frutas.next();
// Object { value: undefined, done: true }
```

Nós temos que olhar parte por parte do código

- nós declaramos a função usando `function*`
- nós usamos a palavra-chave `yield` antes do nosso conteúdo
- nós startamos nossa função usando `.next()`
- o último `.next()` que nós chamamos, nós recebemos um objeto vazio e obtemos `done:true`


Nossa função é pausada entre cada chamada `.next()`.


&nbsp;

## Dar loop sobre um array com um gerador

Nós podemos usar a estrutura de repetição `for of` para iterar sobre o nosso gerador e para `yield` o conteúdo de cada loop.


``` js
// criando um array de frutas
const frutaLista = ['Banana','Maçã','Laranja','Melão','Cereja','Manga'];

// criando nosso loop de gerador
function* loop(arr) {
  for (const fruta of frutaLista) {
    yield `Eu gosto de comer ${fruta}`;
  }
}


const frutaGerador = loop(frutaLista);
frutaGerador.next();
// Object { value: "Eu gosto de comer Banana", done: false }
frutaGerador.next();
// Object { value: "Eu gosto de comer Maçã", done: false }
frutaGerador.next().value;
// "Eu gosto de comer Laranja"
```

- Nosso novo gerador irá repetir o array e printar um valor por vez sempre que for chamado `.next()`.
- se você está preocupado apenas em receber o valor, então use o `.next().value` e ele não vai printar o status do gerador

&nbsp;

## Finalizando o gerador com `.return()`

Usando o `.return()` nós podemos retornar um valor dado e finalizar o gerador.

``` js
function* frutaLista(){
  yield 'Banana';
  yield 'Maçã';
  yield 'Laranja';
}

const frutas = frutaLista();

frutas.return();
// Object { value: undefined, done: true }
```

Nesse caso nós obtivemos `value: undefined` porque nós não passamos nada no `return()`.

&nbsp;

## Tratando erros com `.throw()`


``` js
function* gen(){
  try {
    yield "Tentando...";
    yield "Tentando muito...";
    yield "Tentando muito mais..";
  }
  catch(err) {
    console.log("Erro: " + err );
  }
}

const meuGerador = gen();
meuGerador.next();
// Object { value: "Tentando...", done: false }
meuGerador.next();
// Object { value: "entando muito...", done: false }
meuGerador.throw("ooops");
// Erro: ooops
// Object { value: undefined, done: true }
```

Como você pode ver nós declaramos `.throw()`o `gerador` nos retornou o erro e finalizou embora nós ainda tivéssemos mais um `yield` para executar.

&nbsp;

## Combinando Geradores com Promessas

Como nós vimos anteriormente, Promessas são muito úteis para programação assíncrona, e combinando elas com os geradores nós podemos ter uma poderosa ferramenta a nossa disposição para evitar problemas como o *inferno das callbacks*.

Como nós estamos discutindo apenas o ES6, eu não falarei sobre funções assíncronas como elas são apresentadas no ES8 (ES2017) mas saiba que o jeito que elas trabalham é baseado no que você verá agora. 

Você pode ler mais sobre funções assíncronas no Capítulo 19.

Usar um Gerador em combinação com uma Promessa nos permitirá escrever um código assíncrono que se parece com síncrono.

O que nós queremos fazer é esperar por uma promessa resolver e então passar o valor tratado de volta para o nosso gerador na chamada `.next()`.

``` js
const minhaPromessa = () => new Promise((resolve) => {
  resolve("nosso valor é...");
});

function* gen() {
  let resultado = "";
  // retorna uma promessa
  yield minhaPromessa().then(data => { resultado = data }) ;
  // esperar a promessa e usar seu valor
  yield resultado + ' 2';
};

// Chamar a função assíncrona e passar os parâmetros.
const funcaoAssincrona = gen();
funcaoAssincrona.next();
// chamar a promessa e esperar pela resposta
// {value: Promise, done: false}
funcaoAssincrona.next();
// Object { value: "nosso valor é... 2", done: false }
```

A primeira vez que nós chamamos `.next()` ele vai chamar nossa promessa e vai esperar pela resposta (no nosso exemplo simples ele responde imediatamente) e quando nós chamamos `.next()` novamente ele vai utilizar o valor retornado pela promessa para alguma outr acoisa (nesse caso só interpola uma string).
