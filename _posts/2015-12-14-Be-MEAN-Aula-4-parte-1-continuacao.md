---
layout: post
title: "Be MEAN (MongoDB): Operadores de Array no comando update()"
date: 2015-12-14 20:42:00 -02:00
description: "Continuação do resumo da quarta aula do Be MEAN, sobre modificação de documentos"
tags:
- MongoDB
categories:
- be-mean-mongodb
categories-humanreadable:
- Be MEAN (MongoDB)
---

Continuando o [post anterior onde vimos o comando update()]({% post_url 2015-11-23-Be-MEAN-Aula-4-parte-1 %}) e seus operadores de modificação, hoje vou relatar o que eu aprendi sobre operadores de array.

## Operadores de Array

$push
: Adiciona um valor ao campo, caso o campo seja um *Array* existente
: Caso não exista irá criar um novo do tipo *Array* e adicionar o valor passado
: Se o campo não for do tipo *Array*, retorna um erro
: Uso: `{ $push : { campo : valor } }`

$pushAll
: O mesmo que o item anterior, porém serve para adicionar vários valores ao array
: Uso: `{ $pushAll : { campo : [Array_de_valores] } }`

$pull
: Retira o valor do campo, case seja do tipo *Array*
: Se o campo não existir, não vai fazer nada
: Uso: `{ $pull : { campo : valor } }`

$pullAll
: O mesmo que o item anterior, porém serve para remover vários valores do array
: Uso: `{ $pullAll : { campo : [Array_de_valores] } }`

## Já acabou

Esta na verdade é uma continuação do [último post da série Be MEAN]({% post_url 2015-11-23-Be-MEAN-Aula-4-parte-1 %}). No dia daquele post eu já não podia continuar escrevendo e por isso deixei esta parte pra depois.

Demorei um pouco pra fazer este post, mas agora que estou de férias é hora de decolar com os projetos!
