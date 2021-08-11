# DAY 2 BLOCK 9 - Promises

A primeira parte da aula, a ocupar no máximo 10 minutos, é uma discussão acerca das cinco perguntas dos exercícios de fixação. Escolha uma pessoa para responder cada e, caso haja dificuldade, escolha outra pessoa para ajudar o primeiro. É importante que os conceitos estejam claros.

1. O que é um código que é executado de modo assíncrono? Qual é a diferença disso para um código que é executado de modo síncrono?

    Código assíncrono é aquele que funciona independentemente do resto do processo, ao contrário do código síncrono que roda seguindo a ordem natural do processo.

2. Quando você tem que enfileirar várias _callbacks_, que problema surge?

    Vários callbacks enfileirados fazem com que o código seja dependente de cada passo da cadeia. O código adquire uma maior propensão de quebra, além de dificultar sua leitura, interpretação e, consequentemente, sua manutenção.

3. Porque as _Promises_ são uma forma de se resolver esse problema?

    As _Promises_ organizam a forma de se chamar os callbacks, uma vez que retira-se a necessidade de aninhamento das funções. Além disso ela nos permite adaptar o código para manipulação de erros, uma vez que possibilita a separação de procedimentos em caso de sucesso ou de falha.

4. Quando você vai se comunicar com uma _API_ tal comunicação deve ser síncrona ou assíncrona? Lembre-se que o serviço ao qual você está se conectando pode demorar muito ou pouco para dar retorno, pode estar fora do ar, etc.

    Comunicações com _API's_ devem ser assíncronas, uma vez que o serviço está suscetível a uma diversidade de falhas. Tais falhas, em um código síncrono, podem prejudicar e em algumas situações comprometer seu funcionamento como um todo.

5. A partir da resposta anterior, o que é `fetch()` e pra que ele serve?

    `fetch()` nada mais é que a função base para efetuar requisições http utilizando o JavaScript. Serve para integrar nossas aplicações com outras _API's_.

---

A seguir temos 40 minutos de exposição com 10 minutos para tirar dúvidas. Em função da complexidade do conteúdo essa será uma aula expositiva com dúvidas sendo anotadas via sli.do e tiradas nos últimos dez minutos de aula.

1. Primeiramente vamos nos lembrar do código da última aula. Cole este código em um arquivo, abra-o e execute-o no navegador.

```html
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Live Lecture 9.2</title>
  <script>
    const appendJoke = (joke) => {
      let ul = document.querySelector("ul")
      let li = document.createElement("li")
      li.innerHTML = joke
      ul.appendChild(li)
    }

    const param = { headers: { Accept: 'application/json' } };
    const fetchJoke = () => {
      fetch('https://icanhazdadjoke.com/search?term=spider', param)
        .then((response) =>
          response.json()
            .then((data) => {
              appendJoke(data.results[0].joke);
              fetch("https://icanhazdadjoke.com/search?term=ghost", param)
                .then((response) =>
                  response.json()
                    .then((data) => {
                      appendJoke(data.results[0].joke);
                      fetch("https://icanhazdadjoke.com/search?term=pizza", param)
                        .then((response) =>
                          response.json()
                            .then((data) => {
                              appendJoke(data.results[0].joke);
                              fetch("https://icanhazdadjoke.com/search?term=wolf", param)
                                .then((response) =>
                                  response.json()
                                    .then((data) => {
                                      appendJoke(data.results[0].joke);
                                      fetch("https://icanhazdadjoke.com/search?term=ant", param)
                                        .then((response) =>
                                          response.json()
                                            .then((data) => {
                                              appendJoke(data.results[0].joke);
                                            })
                                        )
                                    })
                                )
                            })
                        )
                    })
                )
            })
        )
    }
    window.onload = () => fetchJoke();
  </script>
</head>

<body>
  <ul>
  </ul>
</body>

</html>
```

2. Repare como está confuso. Suponha que eu queira retirar o termo 'ghost' da lista de piadas sem alterar as urls. Como faço?

  - Peça para uma pessoa que estuda na Trybe te ajudar a fazer.

3. Não é lá muito intuitivo, certo? Agora imagine que cada callback tem lógicas completamente diferentes. Umas longas e outras curtas. Isso fica ruim muito rápido. Como resolvemos isso? _Promises_!

Vamos encapsular essas chamadas para `fetch` em _Promises_!

  - Primeiramente, extraia para uma função a lógica do callback:

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->
  const param = { headers: { Accept: 'application/json' } };
    const getRandomJoke = (term) => {
      fetch(`https://icanhazdadjoke.com/search?term=${term}`,param)
        .then((response)=> 
          response.json()
          .then((data)=> appendJoke(data.results[0].joke))
        );
    } 
     const fetchJoke = () => {
      <!-- fetch('https://icanhazdadjoke.com/search?term=spider', param)
        .then((response) =>
          response.json()
            .then((data) => {
              appendJoke(data.results[0].joke);
              fetch("https://icanhazdadjoke.com/search?term=ghost", param)
                .then((response) =>
                  response.json()
                    .then((data) => {
                      appendJoke(data.results[0].joke);
                      fetch("https://icanhazdadjoke.com/search?term=pizza", param)
                        .then((response) =>
                          response.json()
                            .then((data) => { 
                              appendJoke(data.results[0].joke);-->
                              fetch('https://icanhazdadjoke.com/search?term=wolf', param)
                              .then((response) =>
                                response.json()
                                  .then((data) => {
                                    appendJoke(data.results[0].joke);
                                    getRandomJoke('ant')
                                  })
                              )
                            <!-- })
                        )
                    })
                )
            })
        ) -->
    }
    <!-- window.onload = () => fetchJoke(); -->
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

4. Comente sobre o segundo parâmetro de fetch e dê refresh na página e note que o comportamento não mudou. Beleza! Agora vamos espalhar essa refatoração para os outros termos!

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

    const param = { headers: { Accept: 'application/json' } };
    const getRandomJoke = (term, callback) => {
      fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
        .then((response) =>
          response.json()
            .then((data) => {
              appendJoke(data.results[0].joke)
              callback ? callback() : undefined
            })
        );
    }

    const fetchJoke = () => {
      getRandomJoke("spider",
      getRandomJoke("pizza",
        getRandomJoke("wolf",
          getRandomJoke("ant")
          )
        )
      )
    }
    window.onload = () => fetchJoke();
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

5. Atualize a página para ver que nada mudou, a refatoração funciona. Ficou... menos pior. Mas não está legal. Imagine que eu te peço pra mudar a lógica da piada `"wolf"`. O código já vai ficando confuso outra vez.

Agora... Que tal usarmos _Promises_?! Atente para o comportamento da função `fetch(`. Ela recebe dois parâmetros: a URL com a requisição que deve ser feita e um objeto de opções que já possui um valor padrão caso nada seja passado, para essa _API_ precisamos passar o objeto identificado abaixo, que ja utilizamos desde o inicio, repare no comportamento do `.then`, ele possui uma `callback` que lida com os dados de uma requisição bem sucedida.

  - Mostre no console do navegador o resultado do .then antes e depois do response.json!

  ```javascript
    fetch("https://icanhazdadjoke.com/search?term=wolf", { headers: { Accept: 'application/json' } })
      .then((response)=> console.log(response))
  ```

- Mostre no console que sem o header a requisição abaixo retorna um erro.

 ```javascript
    const fetchJoke = () => {
      fetch("https://icanhazdadjoke.com/search?term=wolf", { headers: { Accept: 'application/json' } })
      .then((response)=> response.json())
      .then((data)=> console.log(data))
    }
  ```

  - Observe que ele imprimiu a piada! Beleza. Nós vamos mudar agora essa _callback_ para que ela faça **outra coisa** quando a requisição for bem sucedida. Agora ela será assim:

  ```javascript
    (data) => {
      appendJoke(data.results[0].joke)
      resolve()
    }
  ```

  - Ou seja! Caso a requisição seja bem sucedida ele primeiro anexa a piada ao DOM e em seguida chama a `resolve` da _Promise_!

6. Digite o código novo abaixo linha a linha, explicando em detalhes.

  - A função `resolve` vai para o `then` da _Promise_. O `then` recebe como parâmetro uma _arrow function_ que retorna a nossa outra _Promise_! E assim sucessivamente. Enquanto estivermos chamando funções que retornam _Promises_ dentro dos `then` podemos continuar encadeando elas!

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

    const param = { headers: { Accept: 'application/json' } };
    const getRandomJokePromise = (term) => {
      return new Promise((resolve, reject) => {
        fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
          .then((response) =>
            response.json()
              .then((data) => {
                appendJoke(data.results[0].joke)
                resolve()
              })
          );
      })
    }

    const fetchJoke = () => {
      getRandomJokePromise("spider")
      .then(() => getRandomJokePromise("pizza"))
      .then(() => getRandomJokePromise("wolf"))
      .then(() => getRandomJokePromise("ant"))
    }
    window.onload = () => fetchJoke();
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

7. Atualize a página e mostre que nada mudou. Abra o código antigo e o código novo lado a lado.

  - E agora, está mais fácil de ler o código ou mais difícil?

  - Se eu precisar mudar algo em alguma das piadas, eu consigo com facilidade ou dificuldade?

  - Essa é a beleza das _Promises!_

8. Como última demonstração, volte com a piada sobre "ghosts" após a piada da aranha!

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

  <!-- const param = { headers: { Accept: 'application/json' } };
      const getRandomJokePromise = (term) => {
        return new Promise((resolve, reject) => {
          fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
            .then((response) =>
              response.json()
                .then((data) => {
                  appendJoke(data.results[0].joke)
                  resolve()
                })
            );
        })
      } -->
  <!-- const fetchJoke = () => { -->
<!--     getRandomJokePromise("spider") -->
      .then(() => getRandomJokePromise("ghost"))
      <!-- .then(() => getRandomJokePromise("pizza")) -->
      <!-- .then(() => getRandomJokePromise("wolf")) -->
      <!-- .then(() => getRandomJokePromise("ant")) -->
      <!-- } -->
    <!-- window.onload = () => fetchJoke(); -->
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

9. E, por fim, o que acontece se alguma dessas _Promises_ retornarem um erro? É só colocar um `catch` ao final do encadeamento e pronto!

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

  <!-- const param = { headers: { Accept: 'application/json' } };
      const getRandomJokePromise = (term) => {
        return new Promise((resolve, reject) => {
          fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
            .then((response) =>
              response.json()
                .then((data) => {
                  appendJoke(data.results[0].joke)
                  resolve()
                })
            );
        })
      } -->
  <!-- const fetchJoke = () => { -->
<!--     getRandomJokePromise("spider") -->
      <!-- .then(() => getRandomJokePromise("ghost")) -->
      <!-- .then(() => getRandomJokePromise("pizza")) -->
      <!-- .then(() => getRandomJokePromise("wolf")) -->
      <!-- .then(() => getRandomJokePromise("ant")) -->
      .catch(error => console.log(error))
      <!-- } -->
    <!-- window.onload = () => fetchJoke(); -->
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

10. Para simular um erro, vamos forçar a _Promise_ a quebrar!

  - Vá trocando os temas de piada e dando refresh na página com o console aberto, para mostrar que o encadeamento vai até dar erro aí para, e não importa onde pare o erro é tratado igualmente.

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

  const param = { headers: { Accept: 'application/json' } };
    const getRandomJokePromise = (term) => {
      return new Promise((resolve, reject) => {
        if (term == "ant") reject('That joke was too good to be here!')
        else {
        fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
          .then((response) =>
            response.json()
              .then((data) => {
                appendJoke(data.results[0].joke)
                resolve()
              })
          );
        }
      })
    }
    <!-- const fetchJoke = () => { -->
    <!-- getRandomJokePromise("spider") -->
      <!-- .then(() => getRandomJokePromise("ghost")) -->
      <!-- .then(() => getRandomJokePromise("pizza")) -->
      <!-- .then(() => getRandomJokePromise("wolf")) -->
      <!-- .then(() => getRandomJokePromise("ant")) -->
      <!-- .catch(error => console.log(error)) -->
      <!-- } -->
    <!-- window.onload = () => fetchJoke(); -->
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

11. Agora vamos usar Async/Await para refatorar o código.

```html
<!-- <html lang="en"> -->
<!-- <head> -->
<!--   <meta charset="UTF-8"> -->
<!--   <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
<!--   <meta http-equiv="X-UA-Compatible" content="ie=edge"> -->
<!--   <title>Live Lecture 9.2</title> -->
<!--   <script> -->
<!--     const appendJoke = (joke) => { -->
<!--       let ul = document.querySelector("ul") -->
<!--       let li = document.createElement("li") -->
<!--       li.innerHTML = joke -->
<!--       ul.appendChild(li) -->
<!--     } -->

  <!-- const param = { headers: { Accept: 'application/json' } };
      const getRandomJokePromise = (term) => {
        return new Promise((resolve, reject) => {
          if (term == "ant") reject('That joke was too good to be here!')
          else {
          fetch(`https://icanhazdadjoke.com/search?term=${term}`, param)
            .then((response) =>
              response.json()
                .then((data) => {
                  appendJoke(data.results[0].joke)
                  resolve()
                })
            );
          }
        })
      } -->

    const fetchJoke = async () => {
      try {
        await getRandomJokePromise("spider")
        await getRandomJokePromise("ghost")
        await getRandomJokePromise("pizza")
        await getRandomJokePromise("wolf")
        await getRandomJokePromise("ant")
      } catch (error) {
        console.log(error)
      }
    }
    <!-- window.onload = () => fetchJoke(); -->
  <!-- </script> -->
<!-- </head> -->
<!-- <body> -->
  <!-- <ul> -->
  <!-- </ul> -->
<!-- </body> -->
<!-- </html> -->
```

12. This is it! Agora abre-se para tirar dúvidas. Vá passando pelas dúvidas do chat e voltando o código pra trás se preciso. Convide as pessoas a levantarem a mão se quiserem e voltarem a cada etapa do raciocínio que não ficou clara para elas.
