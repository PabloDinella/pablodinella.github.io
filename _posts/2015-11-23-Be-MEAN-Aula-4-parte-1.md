---
layout: post
title: "Be MEAN (MongoDB): save(), update() e operadores de modificação"
date: 2015-11-23 00:20:00
description: "Resumo da quarta aula do Be MEAN, sobre modificação de documentos"
tags:
- MongoDB
categories:
- be-mean-mongodb
categories-humanreadable:
- Be MEAN (MongoDB)
---

Rapaz, esse curso tá ficando cada vez mais denso! E isso é bom, afinal quanto mais conhecimento melhor! Vou escrever bastante, então prepare-se:

Nessa aula foi recordado que para alterar um documento no MongoDB, temos dois comandos:

`db.collection.save()` e `db.collection.update()`

## Modificando documentos com `save()`

A diferença básica entre os dois comandos é que com o `save` temos que buscar e guardar o objeto numa variável, e depoooois salvá-lo no banco, dessa forma:

Primeiro buscamos o objeto: `var p = db.pokemons.findOne({name:/pikachu/i})
`

Temos que usar `findOne()` pois o `find()` não retorna o objeto em si, mas sim um *cursor*. Me corrija nos comentários se eu estiver errado!

Então `p` terá o seguinte conteúdo:

~~~
{
  "_id": ObjectId("5650c5c8c36927f637e6ce10"),
  "name": "Pikachu",
  "description": "Rato elétrico bem fofinho",
  "type": "electric",
  "attack": 55,
  "height": 0.4
}
~~~

Agora podemos alterar um de seus valores, como por exemplo: `p.attack = 50`.

E daí salvamos com `db.pokemons.save(p)`. Isso altera o valor do attack do Pikachu no banco.

## Modificando documentos com `update()`

Já com `update()` é um pouco diferente. Primeiro, o `update()` recebe três parâmetros:

- query (geralmente com o [\_id]({% post_url 2015-11-14-Be-MEAN-Aula-3 %}) do documento que queremos modificar)
- update (a modificação)
- options (parâmetro opcional, veremos mais pra frente)

Com esta sintaxe: `db.collection.update(query, update, options)`.

Podemos então criar uma query `var query = {"_id": ObjectId("5652574334d59f7ade6bf57e")}`
que no meu caso retornará este documento:

~~~
{
  "_id": ObjectId("5652574334d59f7ade6bf57e"),
  "name": "Testemon",
  "attack": 8000,
  "defense": 8000,
  "height": 2.1,
  "description": "Pokemon de teste"
}
~~~

e um *mod* (parâmetro `update`):

~~~
var mod = {description: "Teste de modificação"}
~~~

Então passamos os parâmetros ao comando `update()`: `db.pokemons.update(query, mod)`.

Executamos o comando, e ao buscar o documento no banco com `db.pokemons.find(query)` para ver como ficou temos este retorno:

~~~
{
  "_id": ObjectId("5652574334d59f7ade6bf57e"),
  "description": "Teste de modificação"
}
~~~

Opa! O `update()` não só modificou a `description` do nosso documento como removeu os outros campos. Isso acontece porque passamos o parâmetro de *update* sem usar um **operador de modificação**.

De acordo com a [documentação do mongo](https://docs.mongodb.org/manual/reference/method/db.collection.update/#replace-a-document-entirely):

- O comando `update()` substitui o documento que corresponde a nossa `query` pelo documento do nosso segundo parâmetro, no nosso caso o `mod`
- O comando `update()` não altera o \_id do documento
- O comando `update()` não altera múltiplos documentos de uma vez

Então para alterarmos um campo apenas, sem substituir o documento inteiro, temos de usar nossos...

## Operadores de Modificação

Que são:

$set
: muda o valor de um campo, criando-o caso não exista
: uso: `{ $set: { campo: valor } }`

$unset
: remove campo
: uso: `{ $unset: { campo: 1 } }` colocamos **1** (true) para remover o campo

$inc
: incrementa o valor do campo com a quantidade especificada
: para decrementar, usar valor negativo
: uso: `{ $inc: { campo: 1 } }`

Exemplo:

~~~
var mod = {$inc: { attack: 1 }}
db.pokemons.update(query, mod)
~~~

O código acima vai apenas incrementar do campo `attack` do nosso registro em 1, e manter todos os outros campos intactos.

A lista segue, mas para não deixar o post muito longo vou parar por aqui, que é até onde foi abordado [na aula](https://www.youtube.com/watch?v=ONzJsNbv15U). Mas o restante [está na documentação](https://docs.mongodb.org/manual/reference/operator/update/#fields) :)

## No próximo post eu continuo!

Eu disse que o curso estava ficando denso mas nem eu sabia o quanto... Já escrevi tanto e não acabou ainda! Por isso vou parar por aqui, assim não abordo muitos assuntos num post só.

Então assine meu [feed RSS](http://pablodinella.github.io/atom.xml) e/ou me siga no [facebook e aguarde](https://www.facebook.com/pablordinella) ;)
