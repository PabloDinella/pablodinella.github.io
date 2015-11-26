---
layout: post
title: 'Busca "insensitiva" no mongodb, diretivas vs classes no ionic, e kramdown'
date: 2015-11-26 00:02:11 -03:00
description: "Sobre como ignorar maiúsculas e minúsculas no mongodb, classes e diretivas no Ionic Framework, e processador de markdown para Jekyll"
tags:
- Ionic Framework
- MongoDB
- Jekyll
- Markdown
categories:
- blog
categories-humanreadable:
- Blog
---

Seguindo a ideia inicial deste diário, que é anotar o que eu aprendo no dia a dia, aqui vai o que aprendi esta semana ;)

## Buscas no MongoDB sem diferenciação entre maiúsculas e minúsculas

No MongoDB realizamos buscas com o [comando *find()*]({% post_url 2015-11-14-Be-MEAN-Aula-3 %}) utilizando uma *query*, desta forma:

~~~
db.collection.find({Nome: 'MongoDB'})
~~~

Porém neste caso, a busca retornaria apenas resultados cujo valor do campo **nome** fosse exatamente **MongoDB**.

Mas descobri [através de uma aula do Be MEAN](https://youtu.be/ONzJsNbv15U?t=24m32s) que podemos tornar essa busca *case insensitive*, ou seja, fazer o mongo ignorar a diferença entre maiúsculas e minúsculas, usando *regex*.

Então nossa busca com regex ficaria assim: `db.collection.find({Nome: /mongodb/i})`

Isto faz com que obtenhamos documentos com o nome **mongodb**, **MongoDB**, **mongoDB** e assim por diante.

## Qual a diferença entre usar diretivas <ion-*> e classes no Ionic Framework?

Estou [desenvolvendo um app](https://github.com/PabloDinella/BeautyTime) para a faculade com [Ionic](http://ionicframework.com/), e me veio essa dúvida, qual a diferença entre usar `<ul class="list">` e `<ion-list>` por exemplo?

A [resposta veio](http://forum.ionicframework.com/t/ion-vs-div-directives/7524) [dos fóruns](http://forum.ionicframework.com/t/ionic-html-tags-when-use-ion-prefix-and-when-not/3933): com as classes só temos a estilização, já as diretivas nos dão mais funcionalidades.

Este [comentário no código fonte do Ionic](https://github.com/driftyco/ionic/blob/master/js/angular/directive/list.js#L12-L17) exemplifica bem:

> The containing element requires the `list` class and each list item requires the `item` class.
> However, using the ionList and ionItem directives make it easy to support various interaction modes such as swipe to edit, drag to reorder, and removing items.

## Utilizar o kramdown no seu blog Jekyll lhe dá mais opções

Escrevendo um post, senti a necessidade de fazer uma lista de definições e também gostaria de usar notas de rodapé. O Jekyll no GitHub vem com o processador *redcarpet* por padrão.

Mas dando uma googleada descobri que o *kramdown* me dava essas [opções a mais](http://kramdown.gettalong.org/quickref.html) e para utilizar com o jekyll bastava mudar `markdown: redcarpet` para `markdown: kramdown`

## Concluindo

Estou gostando de fazer este diário, especialmente porque eu aprendo muito mais quando eu crio conteúdo do que quando eu consumo. Afinal não quero escrever besteira, então sempre tento dar uma pesquisada antes de falar sobre algo.

E é claro que bate aquela vontadezinha de que alguém leia seu texto, e eu acho que é esse tipo de pretensão que me faz ser menos produtivo com o blog, porque sempre que penso em escrever algo, fico imaginando qual categoria utilizar, como organizar as tags, como não deixar o post simples demais... E todas estas coisas me seguram um pouco na hora de escrever e publicar meus textos.

Mas o jeito é se manter firme na proposta e insistir nela, por isso pode ser que esse diário seja uma pequena bagunça. E afinal desde quando um diário é organizado?

Às vezes acho que escrevo demais para o que este projeto se propõe, mas não consigo parar. Mas pode ser que isso seja bom, vai formar a personalidade diferenciada deste blog :)
