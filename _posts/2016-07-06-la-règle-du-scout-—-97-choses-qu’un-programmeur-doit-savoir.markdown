---
layout: default
title: "La règle du scout — 97 choses qu’un programmeur doit savoir"
date: 2016-07-06 20:08:54
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [La règle du scout](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Uncle Bob](http://programmer.97things.oreilly.com/wiki/index.php/Uncle_Bob).

<!--excerpt-->

Les boy-scouts on une règle : « Toujours laisser le campement plus propre que l'on ne l'a trouvé. » Si on trouve du désordre sur le terrain, on le nettoie sans regarder qui est responsable. On améliore intentionnellement l'environnement pour le prochain groupe de campeurs. Bon en fait, la règle originale, telle que formulée par [Robert Stephenson Smyth Baden-Powell](https://fr.wikipedia.org/wiki/Robert_Baden-Powell), le père du scoutisme, est « Essayez de laisser ce monde un peu meilleur qu'il ne l'était quand vous y êtes venus. »

Et si nous suivions la règle « **Toujours laisser un module plus propre que quand on l'a inspecté** » ? Peu importe qui en est l'auteur, et si nous faisions toujours l'effort, peu importe s'il est mince, pour améliorer le module. Quel serait le résultat ?

Je pense que si nous suivions tous cette règle toute simple, nous verrions la fin de l'impitoyable détérioration de nos systèmes logiciels. À la place, **nos systèmes d'amélioreraient, petit à petit**, au fur et à mesure de leur évolution. Nous verrions également des **équipes** s'occuper du système dans son entier, plutôt que des individus ne se souciant que de leur petite partie.

Je ne pense pas que ce soit trop demander. Tu n'es pas obligé de quitter chaque module en l'ayant rendu parfait. Tout ce que tu as à faire, c'est le rendre **un peu meilleur** que quand tu l'as examiné. Bien entendu, cela implique que **tout code que tu ajoutes doit être propre**. Cela veut également dire que tu dois **nettoyer au moins un élément**. Tu peux simplement améliorer le nommage d'une variable, ou découper une grosse fonction en deux plus petites. Tu peux casser une dépendance circulaire, ou ajouter une interface pour découpler l'abstraction de l'implémentation.

Franchement, c'est juste le minimum du savoir-vivre pour moi — de la même manière qu'on se lave les mains après être allé aux toilettes, ou qu'on jette ses déchets à la poubelle plutôt que le laisser par terre. En effet, laisser du désordre dans le code devrait être aussi socialement inacceptable qu'abandonner sauvagement ses déchets. Ça doit être quelque chose **qui ne se fait tout simplement pas**.

Mais il y a plus que ça. S'occuper de son code est une chose. Se préoccuper du code de l'équipe en est une autre bien différente. Les équipes s'entraident les uns les autres et nettoient après le passage des autres. Elles suivent la règle d'or du scoutisme parce que c'est **bénéfique pour tout le monde**, pas que pour eux.

## Mon mot à moi

Pas évident à faire. On est parfois pressé et on code mal. Mais on devrait essayer d'éliminer ces mauvaises décisions, cette dette technique justement en améliorant le code qu'on lit. Et on lit bien plus souvent qu'on écrit. Malgré de mauvais choix, la qualité devrait toujours augmenter.

Maintenant, à moi de voir si je vais réussir à appliquer ces principes au travail, dans une base de code qui en aurait bien besoin.

**PS** : on retrouve ce principe dans le livre de Bob, « **Clean Code** », dont le chapitre en question est très bien résumé sur le [blog d'Octo](http://blog.octo.com/scout-toujours/), avec en prime un exemple en Java.
