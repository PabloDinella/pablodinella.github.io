---
layout: post
title: "Be MEAN (MongoDB): ID, find(), e operadores"
date: 2015-11-14 01:00:00 -03:00
description: "Resumo da terceira aula do Be MEAN, sobre UUD, buscas com o find() e operadores"
tags:
- MongoDB
categories:
- be-mean-mongodb
categories-humanreadable:
- Be MEAN (MongoDB)
---

Mais uma aula do Be MEAN, e hoje aprendemos o que seria a segunda operação básica em banco de dados, ou seja, o *READ* do *CRUD*.

Aprendemos dois comandos para recuperar dados no mongodb:

- `db.find()`
- `db.findOne()`

## O UUD (Universal Unique Identifier) de um objeto

Quando inserimos um objeto no mongodb, é gerado um \_id automaticamente (e não tem nada de autoincremento). O \_id tem esse formato:

- 4-bytes que representam os segundos desde a época Unix,
- 3-bytes para o identificador de máquina,
- 2-bytes com o ID do processo,
- e 3-bytes para um contador, começando com um valor aleatório.

Na prática, o \_id se parece com isto: `507f1f77bcf86cd799439011`

Se fosse escrito em binário, seria um número de 96 dígitos. Tipo assim: `010100000111111100011111011101111011110011111000011011001101011110011001010000111001000000010001`

Uma coisa que pensei nesse momento foi: até quantos anos será que dá pra usar esse esquema de 4 bytes para represntar os segundos do UNIX? Pelos meus cálculos, dá 21 séculos (me corrija se eu estiver errado), ufa, dá pra usar este esquema por muito tempo então.

## Voltando ao comando find()

O find aceita dois parâmetros, query (cláusulas) e fields (campos), dessa maneira:

`db.collection.find({clausulas}, {campos})`

Para facilitar, é recomendável criar variáveis especificando um JSON para fazer a busca, desse jeito:

`var query = {Nome: 'Fulano'}`

Daí passamos o parâmetro pro find assim:

`db.collection.find(query)`

O parâmetro fields define quais campos queremos. Para usá-lo devemos dizer o nome do campo junto com o valor **1**, se quisermos que a busca retorne este campo, ou **0** caso não queiramos este campo.

Por exemplo, caso queiramos fazer uma busca que retorne os campos Nome e Idade de um objeto:

`var fields = {Nome: 1, Idade: 1}`
`db.collection.find(query, fields)`

O retorno seria algo como:

~~~
{
  "_id": ObjectId("564220f0613f89ac53a7b5d0"),
  "Nome": "Fulano",
  "Idade": "25"
}
~~~

Perceba que o \_id foi retornado mesmo sem ter sido especificado. É porque ele é o único campo que é retornado sempre. Caso não queiramos que ele seja retornado, basta especificá-lo com o valor 0:

`var fields = {Nome: 1, Idade: 1, _id: 0}`

## Operadores Aritméticos e Lógicos

Os operadores servem para obtermos apenas resultados que satisfaçam certa condição. Temos os seguintes operadores:

- $lt (less than)
- $lte (less than or equal)
- $gt (greater than)
- $gte (greater than or equal)

E dentre os lógicos temos:

- $or
- $nor ("ou sqn")
- $and

E o uso destes segue o formato dos seguintes exemplos:

- `var query = {height:{$lte:0.5}}` para obter apenas resultados cujo campo `height` seja menor ou igual a `0.5`
- `var query = {name:"bob", $nor:[{a:1}, {b:2}]}` para obter resultados que tenham o campo `name` porém os valores de pelo menos um dos campos `a` e `b` **não** sejam `1` e `2` respectivamente.

Pode parecer meio difícil de entender, mas é só ler com calma e/ou mais de uma vez que fica fácil ;)

O JSON desse último exemplo, numa forma tabulada ficaria assim:

~~~
{
  "name": "bob",
  "$nor": [
    {
      "a": 1
    },
    {
      "b": 2
    }
  ]
}
~~~

Exemplo com o operador **$and**:

~~~
var query1 = { a: 1 };
var query2 = { b: { $gt: 5 } } ;

db.collection.find( { $and: [query1, query2] } )
~~~

Nesse caso somente são retornados resultados que satisfaçam as duas condições `query1` e `query2`.

## Operador existencial

Temos também o operador `$exists`, que retorna o objeto caso o campo exista. Por exemplo:

`db.collection.find( { altura : { $exists : true } } );
`

Só retorna objetos nos quais existam o campo `altura`.

## Conclusão

Basicamente esse foi o conteúdo da terceira aula. Espero que tenha sido útil pra você, e qualquer coisa comente abaixo ;)
