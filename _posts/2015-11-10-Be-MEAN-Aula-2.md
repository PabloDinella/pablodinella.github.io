---
layout: post
title: "Be MEAN #2: Mais comandos do MongoDB"
date: 2015-11-10 20:00:00
description: "O que eu aprendi na primeira aula do Be MEAN"
tags:
- mongodb
categories:
- Be MEAN
---

Segunda aula de MongoDB e a coisa está ficando séria! Vou até fazer uma lista de comandos:

## Comandos de hoje
- ```use <collection>``` muda a base de dados em uso, cria se ainda não existir
- ```db``` variável que aponta para a base de dados em uso
- ```show dbs``` mostra as bases criadas
- ```show collections``` mostra as coleções da base de dados em uso
- ```db.createCollection()``` cria uma coleção vazia
- ```db.collection.insert()``` (onde *collection* é a sua coleção) insere objetos
- ```db.collection.find()``` executa uma query
- ```db.collection.save()``` insere e salva
- ```db.collection.findOne()``` executa uma query e retorna um objeto

Obs.: ao criar uma base de dados com o ```use```, e este criar uma nova base de dados, ela não será exibida pelo ```show dbs```, pois nada foi inserido nela ainda.

## Cursors
Ao dar ```var cur = db.collection.find()```, ```cur``` será um cursor, e caso haja documentos que possam ser iterados, podemos fazer um ```while(cur.hasNext()){print(tojson(cur.next()))}``` para mostrar os documentos.

Basicamente (pelo que entendi), NoSQL segue um modelo não relacional, e por isso pode tratar cada caso de forma mais eficiente, existem por exemplo bancos NoSQL para tratar de grafos, de docuemtos, etc.

## Referência
Com tanto conteúdo novo, a [documentação do mongo](https://docs.mongodb.org/manual/reference/) será sua melhor amiga :)
