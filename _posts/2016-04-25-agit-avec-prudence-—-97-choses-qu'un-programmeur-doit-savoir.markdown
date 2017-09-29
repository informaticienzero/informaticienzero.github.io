---
layout: default
title: "Agit avec prudence — 97 choses qu'un programmeur doit savoir"
date: 2016-04-25 19:37:20
permalink: /:title/
---
Je suis tombé très récemment sur [ce site](http://programmer.97things.oreilly.com/wiki/index.php/97_Things_Every_Programmer_Should_Know) intitulé « **97 Things Every Programmer Should Know** ». Il a pour but de regrouper des conseils et suggestions de codeurs expérimentés. Il est disponible sous licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/"). Puisque la licence me le permet, je vais donc traduire ce livre. Mes articles seront donc tous sous cette même licence. Le premier article est [Agit avec prudence](http://programmer.97things.oreilly.com/wiki/index.php/Act_with_Prudence), par [Seb Rose](http://programmer.97things.oreilly.com/wiki/index.php/Seb_Rose "Seb Rose").

<!--excerpt-->

## Agit avec prudence

Même si le planning semble confortable quand l'itération démarre, on sait qu'à un moment ou un autre, on sera sous pression. Dans ces cas-là, on est face au dilemme entre « faire bien » et « faire vite » et bien souvent, c'est ce dernier qui l'emporte et qu'on s'empresse de justifier en arguant qu'on repassera plus tard faire les choses proprement. Et quand cette promesse est faite, à soi-même, à son équipe ainsi qu'au client, c'est qu'on pense sincèrement la tenir. Mais bien trop souvent, l'itération suivante vient avec son lot de problèmes qui focalisent l'attention. Ce travail sans cesse reporté est connu sous le nom de **dette technique** et n'est pas notre ami. C'est ce que Martin Fowler nomme, dans sa [taxonomie de la dette technique](http://martinfowler.com/bliki/TechnicalDebtQuadrant.html), la dette technique délibérée, en opposition à la dette technique accidentelle (NDT : *inadvertent technical debt*).

La dette technique fonctionne comme un emprunt : sur le court-terme c'est avantageux, mais **il faut payer des intérêts jusqu'à ce qu'il soit remboursé**. Ces raccourcis dans le code rendent l'ajout de fonctionnalités ou la refactorisation plus difficiles. Ce sont des nids à problèmes et rendent les cas de tests complexes. Plus on la laisse de côté, plus la situation empire. Avec le temps, quand on cherche enfin à corriger un problème, on peut se retrouver avec tout un tas de choix de design pas tout à fait correct venus se superposer au problème d'origine et qui rendent le refactoring et la correction beaucoup plus complexes. En fait, bien souvent, c'est quand les choses sont à tel point catastrophique qu'on se décide à régler le problème. Mais cette correction est tellement dure à implémenter qu'**on ne veut pas prendre le risque ni le temps de le faire**.

Il y a forcément des moments où l'on accepte de subir cette dette pour tenir les délais ou pour implémenter une partie d'une fonctionnalité. Essaye de ne pas te retrouver dans ce cas ; si la situation l'exige vraiment, alors vas-y. Mais, MAIS tu dois suivre cette dette technique à la trace et la rembourser le plus rapidement possible, sous peine de voir les choses rapidement se dégrader. Dès que tu acceptes ce compromis, note-le sur une *task card* ou créé une issue pour t'assurer qu'elle ne soit pas oubliée.

Si la dette est réglée à l'itération suivante, le coût aura été faible. Par contre, **laisser la dette traîner ne va faire qu'augmenter les intérêts**, qui doivent être suivis pour garder à l'esprit le coût. Par ce moyen, on souligne le coût de la dette technique sur la *business value* du projet et permet de mieux prioriser son remboursement. Le calcul de la dette et la trace des intérêts dépendront du projet, mais tu dois la suivre.

**Rembourse la dette technique dès que possible**. Ne pas le faire serait imprudent.

## Mon mot à moi

Je ne peux que confirmer. Au travail, le projet sur lequel je travaille traîne une dette vieille de 5 ans voire 10 ans sur certains morceaux de code. J'ai tenté d'ajouter des tests unitaires mais je n'ai pas pu. Trop de parties trop fortement couplées, des fonctions de plusieurs centaines de lignes parfois, des cas particuliers qui se multiplient. On essaye donc d'améliorer petit à petit par du refactoring et des tests unitaires.
