# Capítulo 15: Proxie

## O que é um Proxy?

Segundo o MDN:

> O objeto Proxy é usado para definir um comportamento personalizado para operações fundamentais (e.g. 

> the Proxy object is used to define custom behavior for fundamental operations (e.g. propriedade lookup, atribuição, enumeração, função invocação, etc).

&nbsp;

## Como usar uma `Proxy` ?

É assim que nós criamos um Proxy:

``` js
var x = new Proxy(target,handler)
```

- nosso `target` pode ser qualquer coisa, seja objeto, função, ou outro `Proxy`
- um `handler` é um objeto que vai definir o comportamento do nosso `Proxy` quando um operador é executado nele

``` js
// nosso objeto
const cachorro = { raca: "Pastor Alemão", idade: 5}

// nosso Proxy
const cachorroProxy = new Proxy(cachorro, {
  get(target,raca){
    return target[raca].toUpperCase();
  },
  set(target, raca, valor){
    console.log("mudando a raça para...");
    target[raca] = valor;
  }
});

dogProxy.breed;
// "PASTOR ALEMÃO"
cachorroProxy.raca = "Labrador";
// mudando a raca para...
// "Labrador"
cachorroProxy.raca;
// "LABRADOR"
```

Quando nós chamamos o método `get` nós damos um passo adentro do fluxo normal e mudamos o valor da raça para maiúsculo.

Ao definir o novo valor nós damos outro passo e mostramos uma mensagem curta antes de setar o valor.

Proxies podem ser muito úteis, por exemplo se nosso objeto é um número de telefone.

Você pode pegar o valor dado pelo usuário e formatá-lo para corresponder à formatação padrão do seu país.
