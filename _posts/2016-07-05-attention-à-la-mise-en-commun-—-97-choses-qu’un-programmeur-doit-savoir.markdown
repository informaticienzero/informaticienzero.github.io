---
layout: default
title: "Attention à la mise en commun — 97 choses qu’un programmeur doit savoir"
date: 2016-07-05 21:48:05
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Attention à la mise en commun](http://programmer.97things.oreilly.com/wiki/index.php/Beware_the_Share) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Udi Dahan](http://programmer.97things.oreilly.com/wiki/index.php/Udi_Dahan).

<!--excerpt-->

C'était mon premier projet dans l'entreprise. Je venais juste d'obtenir mon diplôme et j'étais anxieux à l'idée de prouver ma valeur, restant tard tous les jours, à parcourir le code existant. Tandis que je travaillais sur ma première fonctionnalité, j'ai pris un grand soin à mettre en place tout ce que j'avais appris — commentaires, logging, déporter le code partagé dans des bibliothèques chaque fois que possible, etc. La revue de code, pour laquelle je me sentais fin prêt fut une dur retour à la réalité — la réutilisation de ce que j'avais appris était mal vue !

Comment cela était-il possible ? Toutes les connaissances acquises à l'université étaient tenues comme la quintessence d'une ingénierie logicielle de qualité. Tous les articles que j'avais lu, les manuels, les développeurs chevronnés qui m'avaient appris. Tout ceci était-il donc faux ?

En fait, je manquais quelque chose d'essentiel. **Le contexte**.

Le fait que deux parties du système, très différentes l'une de l'autre, puissent présenter des similarités dans leur logique et la façon dont elle est mise en œuvre n'était pas aussi significatif que ce que je pensais. Avant que je sorte ces bibliothèques contenant ce fameux code partagé, ces parties n'étaient pas dépendantes l'une de l'autre. Chacune pouvait évoluer indépendamment. Chacune pouvait changement sa logique, son fonctionnement pour s'accorder aux changements du système dans sa globalité. Ces quatre lignes de code similaires n'étaient qu'accidentelles — une anomalie temporelle, une coïncidence. Tout du moins avant que je n'intervienne.

Les bibliothèques que j'ai créé nouaient les lacets d'une chaussure à l'autre. Un pas dans un des *business domain* ne se faisait qu'après s'être synchronisé avec l'autre. Les coûts de maintenance de ces fonctions indépendantes étaient négligeables, mais la bibliothèque commune imposa plus de tests.

Tandis que j'avais diminué le nombres de lignes de code du système, j'en avais augmenté le nombre de dépendances. Le contexte de ces dépendances est crucial — si elles sont localisées, c'est sans doute qu'il y a une justification et elles peuvent apporter une plus-value. (NdT : veuillez me pardonner, mais là je n'ai aucune idée de comment traduire la phrase qui suit, donc elle restera en anglais). *When these dependencies aren't held in check, their tendrils entangle the larger concerns of the system even though the code itself looks just fine*.

Ces erreurs sont pernicieuses parce que, au fond, elles semblent être une bonne idée. Quand elles sont appliquées dans le bon contexte, ces techniques sont précieuses. Dans un mauvais contexte, elles augmentent le coût plus que les bénéfices. Quand je débarque dans une base de code existante, sans connaissance du contexte d'utilisation des différentes parties, je me montre désormais beaucoup plus prudent à propos de mise en commun.

**Fais attention à la mise en commun. Regarde d'abord le contexte. Ensuite, agis.**
