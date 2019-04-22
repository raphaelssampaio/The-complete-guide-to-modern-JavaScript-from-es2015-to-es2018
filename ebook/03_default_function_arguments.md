# Capítulo 3: Argumentos de função padrão

## Argumentos de função padrão

ES6 tornou muito fácil setar argumentos de função padrão. Vamos ver em um exemplo:

``` javascript
function calcularPreco(total, taxa = 0.1, gorjeta = 0.05){
// Quando nenhum valor é dado para taxa ou gorjeta, o padrão 0.1 e 0.05 serão usados
return total + (total * taxa) + (total * gorjeta);,
}
```

E se nós não quisermos passar o parâmetro, dessa forma:

``` javascript
// O 0.15 será sujeito ao segundo argumento taxa, mesmo se nossa intenção for setar 0.15 para a gorjeta
calculatePrice(100, 0.15)
```

Nós podemos resolver fazendo isso:

``` javascript
// Nesse caso 0.15 será vinculado a gorjeta
calculatePrice(100, undefined, 0.15)
```

Funciona, mas não é tão legal, como melhorar?

Com **desestruturação** nós podemos escrever isso:

``` javascript
const Bill = calcularPreco({ gorjeta: 0.15, total:150 });
```

Nós não temos nem que passar os parâmetros na mesma ordem conforme nós declaramos nossa função, uma vez que nós as chamamos da mesma forma que os argumentos o JavaScript saberá como lidar com elas.

Não se preocupe com a desestruturação, nós falaremos sobre ela no Capítulo 6.
