# Capítulo 13: Promessa

## O que é uma Promessa (Promise)?

De acordo com o MDN:
> Uma Promise é um objeto que representa a eventual conclusão ou falha de uma operação assincrona.

JavaScript trabalha quase que inteiramente de modo assíncrono, o que significa que quando nós estamos recebendo algo de uma API, por exemplo, nosso código não vai parar de executar. Veja este exemplo para entender o que vai acontecer:

```js
const data = fetch('sua-url-da-api-vai-aqui');
console.log('Finalizado');
console.log(data);
```

O código não vai parar uma vez que ele atinge o fetch, portanto nosso próximo `console.log` vai ser executado antes de nós recebermos algum valor no retorno, o que significa que o `console.log(data)` será vazio.

Para evitar isso nós usaríamos **callbacks** ou **primises**.

&nbsp;

### O inferno do callback

Talvez você ouviu algo chamado de **inferno do callback** que parece algo assim:

``` js
fs.readdir(source, function (err, files) {
  if (err) {
    console.log('Error finding files: ' + err)
  } else {
    files.forEach(function (filename, fileIndex) {
      console.log(filename)
      gm(source + filename).size(function (err, values) {
        if (err) {
          console.log('Error identifying file size: ' + err)
        } else {
          console.log(filename + ' : ' + values)
          aspect = (values.width / values.height)
          widths.forEach(function (width, widthIndex) {
            height = Math.round(width / aspect)
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(dest + 'w' + width + '_' + filename, function(err) {
              if (err) console.log('Error writing file: ' + err)
            })
          }.bind(this))
        }
      })
    })
  }
})
```

Nós tentamos escrever nosso código de um jeito onde as execuções aconteçam visualmente do topo à parte inferior, causando aninhamentos excessivos nas funções e o resultado é o que você pode ver acima. 

Para melhorar suas callbacks você pode ler este [artigo](http://callbackhell.com/). *(Em inglês somente)*

Aqui nós focaremos em como escrever promessas (promises).

&nbsp;

## Crie sua própria promessa

```js
const minhaPromessa = new Promessa((resolve, reject) => {
  // seu código vai aqui
});
```

É assim que você cria sua própria promessa, `resolve` e `reject` serão chamados uma vez que a promessa é finalizada.

Nós podemos imediatamente retornar isso para ver o que nós obteríamos:

```js
const minhaPromessa = new Promessa((resolve, reject) => {
  resolve("O valor que nós obtivemos da promessa");
});

minhaPromessa.then(
  data => {
    console.log(data);
  });
// O valor que nós obtivemos da promessa
```

Imediatamente nós finalizamos nossa promessa e vemos o resultado no console.

Nós podemos combinar um `setTimeout()` para esperar um certo período de tempo antes de finalizar.

```js
const minhaPromessa = new Promessa((resolve, reject) => {
  setTimeout(() => {
      resolve("O valor que nós obtivemos da promessa");
    }, 2000);
});

minhaPromessa.then(
  data => {
    console.log(data);
  });
// depois de 2 segundos nós iremos obter:
// O valor que nós obtivemos da promessa
```

Esses dois exemplos são bem simples mas **promessas (promises)** são bem úteis quando lidamos com grandes requisições de dados.

No exemplo acima nós o mantivemos simples e somente finalizamos nossa promessa mas na realidade você também irá encontrar erros, então vamos ver como lidar com eles:

``` js
const minhaPromessa = new Promessa((resolve, reject) => {
  setTimeout(() => {
      reject(Error("este é o nosso erro"));
    }, 2000);
});

minhaPromessa
  .then(data => {
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  })
  // Error: este é o nosso erro
  // Stack trace:
  // minhaPromessa</<@debugger eval code:3:14
```

Usamos `.then()` para pegar o valor quando a promessa é finalizada e `.catch()` quando a promessa falha.

Se você ver o log do nosso erro você pode notar que ele nos fala onde o erro aconteceu, isso porque nós escrevemos `reject(Error("este é o nosso erro"));` e não simplesmente `reject("este é o nosso erro");`.

&nbsp;

### Encadeando promessas

Nós podemos encadear promessas uma antes da outra, usando o que foi retornado da anterior como base para a seguinte, se a promessa for finalizada ou se houve falha.

``` js
const minhaPromessa = new Promessa((resolve, reject) => {
  resolve();
});

minhaPromessa
  .then(data => {
    // pega o dado retornado e chama uma função nele
    return facaAlgumaCoisa(data);
  })
  .then(data => {
    // console.log do dado que nós obtemos da promessa anterior
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  })
```

Nós chamamos uma função (ela pode fazer o que você quiser, nesse caso ela não faz nada) e nós passamos o valor para o próximo passo onde nós damos um console.log nele.

Você pode encadear quantas promessas você quiser e o código ainda será mais legível e menor do que esse que nós vimos acima no **inferno do callback**.

Nós não estamos limitados a encadeamento no caso de sucesso, nós também podemos encadear quando nós recebemos uma `falha (reject)`.

```js
const minhaPromessa = new Promessa((resolve, reject) => {
  resolve();
});

minhaPromessa
  .then(data => {
    throw new Error("ooops");

    console.log("primeiro valor");
  })
  .catch(() => {
    console.log("pegar um erro");
  })
  .then(data => {
  console.log("segundo valor");
  });
  // pegar um erro
  // segundo valor
```

Nós não obtivemos o "primeiro valor" pois nós lançamos um erro, portanto nós só obtivemos o primeiro `.catch()` e o último `.then()`.

&nbsp;

### `Promise.resolve()` & `Promise.reject()`

`Promise.resolve(`) e `Promise.reject()` criarão promessas que automaticamente finalizam ou falham.

```js
//Promise.resolve()
Promise.resolve('Sucesso').then(function(valor) {
  console.log(valor); 
  // "Sucesso"
}, function(valor) {
  // não chamado
});

// Promise.reject()
Promise.reject(new Error('falha')).then(function() {
  // não chamado
}, function(error) {
  console.log(error);
  // Error: falha
});
```

&nbsp;

### `Promise.all()` & `Promise.race()`

`Promise.all()` retorna uma única Promessa que finaliza quando todas as promessas são finalizadas.

Vamos ver um exemplo onde nós temos duas promessas.

```js
const promessa1 =  new Promessa((resolve,reject) => {
  setTimeout(resolve, 500, 'primeiro valor');
});
const promessa2 =  new Promessa((resolve,reject) => {
  setTimeout(resolve, 1000, 'segundo valor');
});

promessa1.then(data => {
  console.log(data);
});
// depois de 500 ms
// primeiro valor
promessa2.then(data => {
  console.log(data);
});
// depois de 1000 ms
// segundo valor
```

Elas finalizarão independentemente de uma da outra mas veja o que acontece quando usamos `Promisse.all().`

```js
Promise
  .all([promessa1, promessa2])
  .then(data => {
    const[promessa1data, promessa2data] = data;
    console.log(promessa1data, promessa2data);
  });
// depois de 1000 ms
// primeiro valor segundo valor
```

Nossos valores retornaram juntos, depois de 1000ms (o timeout da segunda promessa) significando que a primeira teve que esperar a segunda finalizar.

Se nós estivermos passando uma iteração vazia então ele já retornará uma promessa finalizada.

Se uma das promessa falhar, todas elas assincronamente falharão e retornarão o valor da falha, não importa se elas tiverem sido finalizadas.

```js
const promessa1 =  new Promessa((resolve,reject) => {
  resolve("meu primeiro valor");
});
const promessa2 =  new Promessa((resolve,reject) => {
  reject(Error("oooops erro"));
});

// uma das promessas irá falhar, mas .all só retornará uma falha.
Promessa
  .all([promessa1, promessa2])
  .then(data => {
    const[promessa1data, promessa2data] = data;
    console.log(promessa1data, promessa2data);
  })
  .catch(err => {
    console.log(err);
  });
  // Error: oooops erro
```

Por outro lado o `Promise.race()` retorna uma promessa que finaliza ou falha tão logo uma das promessas na iteração finaliza ou falha, com o valor daquela promessa.

``` js
const promessa1 =  new Promessa((resolve,reject) => {
  setTimeout(resolve, 500, 'primeiro valor');
});
const promessa2 =  new Promessa((resolve,reject) => {
  setTimeout(resolve, 100, 'segundo valor');
});

Promessa.race([promessa1, promessa2]).then(function(valor) {
  console.log(valor);
  // Ambos finalizaram, mas a promessa2 é mais rápida
});
// output esperado: "segundo valor"
```

Se nós passarmos um iterável vazio, o race ficará pendente para sempre!.
