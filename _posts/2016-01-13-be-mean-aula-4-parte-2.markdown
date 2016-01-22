---
layout: post
title: "Be MEAN (MongoDB): Sobre upsert, multi, e operadores de query"
modified:
categories: be-mean-mongodb
categories-humanreadable: Be MEAN (MongoDB)
description:
tags: [MongoDB]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2016-01-13T20:36:05-02:00
---

## Olá de volta!

Acredita que no dia que peguei pra assistir essa aula e escrever esse resumo, eu demorei 1 hora para assistir 12 minutos?

Pois é, escrever esses resumos está tomando muito tempo! Por um lado é bom, porque quanto mais você se ocupa com um assunto melhor você aprende ele (vi um vídeo muito legal sobre isso ontem: [https://youtu.be/PHnBUw1bUCU](https://youtu.be/PHnBUw1bUCU)).

Porém infelizmente não acho que vou conseguir continuar fazendo isso até o final do curso. Parece que estou traduzindo a documentação do MongoDB. Aliás, escrever essa documentação deve dar um trabalho incrivelmente demorado, parabéns a quem faz isso!

Meu objetivo é pelo menos terminar de fazer os resumos do módulo de MongoDB, e aí arrumar outra estratégia para resumir, talvez com **mapas mentais** :)

Mas até lá, vou continuar escrevendo ;) Aliás, agora me sinto mais satisfeito, pois posso dizer que estou cumprindo mais uma [meta para o blog em 2016]({% post_url 2016-01-08-Ano-Novo %}) :D

## Começando a aula

Relembrando [a sintaxe do comando `update()`]({% post_url 2015-11-23-Be-MEAN-Aula-4-parte-1 %}), temos:

{% highlight javascript %}
db.collection.update(query, update, options)
{% endhighlight %}

E hoje veremos mais sobre o parâmetro *options*.

## options

**options** é um objeto com a seguinte sintaxe:

{% highlight javascript %}
{
   upsert: <boolean>,
   multi: <boolean>,
   writeConcern: <document>
 }
{% endhighlight %}{

upsert
: Caso o objeto da sua *query* não seja encontrado para ser modificado, ele será criado automaticamente pelo update()
: Valor padrão: *false*

Então se formos mudar um campo de um objeto e ele não encontrar esse objeto, o mongo irá criar um novo objeto com esse campo e valor, **porém só com esse campo e valor que pedimos para modificar**. Por exemplo, vamos buscar um pokemon com um nome inexistente e alterar o campo `active`:

{% highlight javascript %}
var query = {name: /inexistente/i}
var mod = {$set:{active:true}}
var options = {upsert:true}
db.pokemons.update(query,mod,options)
{% endhighlight %}


O resultado será:

{% highlight javascript %}
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("5696d7c5d37af6f34881ad92")
})
{% endhighlight %}


Porém o objeto criado só contém:

{% highlight javascript %}
{
  "_id": ObjectId("5696d7c5d37af6f34881ad92"),
  "active": true
}
{% endhighlight %}


Eu mesmo na primeira vez esperei que o mongo adicionasse o objeto com `name: inexistente`, mas ele só insere os campos do parâmetro `mod`.

## Inserindo valores padrão para novos objetos ($setOnInsert)

Caso o `upsert` esteja ligado e o mongo não encontre o objeto a ser modificado, ele vai criar um novo com os campos e valores passados para modificação. Com o operador `$setOnInsert`, nós podemos setar valores padrão para o mongo usar nos novos objetos:

{% highlight javascript %}
var query = {name: /NaoExisteMon/i}
var mod = {
  $set: {active: true},
  $setOnInsert: {
    name: "NaoExisteMon",
    attack: null,
    defense: null,
    height: null,
    description: "Sem maiores informações"}
}
var options = {upsert: true}
db.pokemons.update(query, mod, options)
{% endhighlight %}


O código acima vai alterar o campo **active** do objeto com **name: NaoExisteMon**, e caso o objeto não exista ele vai criar com os campos **name, attack, defense, height, description** e **active**, com seus respectivos valores.

## Alterando múltiplos documentos

No MongoDB, caso você queira alterar mais de um documento, utilizando apenas uma query, é necessário mudar o **multi** para **true**. Como vimos [mais acima](#options), o multi faz parte das *options*.

Então caso queiramos inserir o campo **active** em todos os pokemons (e alterá-los para **false** caso já exista), fazemos:

{% highlight javascript %}
var query = {}
var mod = {
  $set: {
    active: false
  }
}
var options = {
  multi: true
}
db.pokemons.update(query, mod, options)
{% endhighlight %}

## Voltando ao find()

### Operador $in

Nesse momento da aula foi apresentado o operador **$in**, do comando find(). Vamos direto ao exemplo:

{% highlight javascript %}
var query = {moves:{$in: [/choque do trovão/i]}}
db.pokemons.find(query)
{% endhighlight %}

O operador $in vai retornar os documentos que possuírem os itens pedidos. No caso o campo **moves** é um array, e a busca vai retornar todos os documentos que tiverem o item **choque do trovão** no campo **moves**.

### Operador $nin

E é claro que temos o operador **$nin** (not in), que faz justamente o inverso, retorna os documentos que não tiverem nenhum dos valores passados.

### Operador $all

Se você conhece programação deve estar imaginando que também temos um operador tipo AND, e temos: **$all**.

Esse operador só retorna documentos que tenham todos os itens passados.

### Operador $ne

Continuando a sequência de operadores, temos o **$ne** (not equal), que como se pode supor, retorna documentos cujo o valor do campo seja **diferente** do passado na query. Exemplo:

{% highlight javascript %}
var query = {type: {$ne: 'grama'}}
db.pokemons.find(query)
{% endhighlight %}

Retorna todos os pokemons que não são do tipo grama.

**Obs.:** esse operador não aceita regex, caso tente usar algo como `var query = {name: {$ne: /pikachu/i}}`, vai dar o seguinte erro:

{% highlight javascript %}
Error: error: {
  "$err": "Can't canonicalize query: BadValue Can't have regex as arg to $ne.",
  "code": 17287
}
{% endhighlight %}

### Operador $not

Quase o mesmo que o operador anterior, porém inclui documentos que não contenham o campo especificado, e também permite regex.

## Removendo documentos

### remove()

O comando remove() remove todos os documentos encontrados na query, por exemplo:

{% highlight javascript %}
var query = {name: \squirtle\i}
db.pokemons.remove(query)
{% endhighlight %}

### drop()

O drop() serve para remover uma coleção inteira :)

## Leia a documentação!

Lembre-se de que todos os tópicos aqui descritos estão melhor explicados na [documentação oficial do MongoDB](https://docs.mongodb.org/manual/) ;)
