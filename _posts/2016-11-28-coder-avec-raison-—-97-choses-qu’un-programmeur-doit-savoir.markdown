---
layout: default
title: "Coder avec raison — 97 choses qu’un programmeur doit savoir"
date: 2016-11-28 15:07:32
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Coder avec raison](http://programmer.97things.oreilly.com/wiki/index.php/Coding_with_Reason) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Yechiel Kimchi](http://programmer.97things.oreilly.com/wiki/index.php/Yechiel_Kimchi).

<!--excerpt-->

Tenter de raisonner à propos de la *software correctness* (NdT : terme qu'on pourrait traduire par « exactitude logicielle »)  manuellement finit en une preuve formelle plus longue que le code et susceptible de contenir plus d'erreurs que le code. Les outils automatiques sont préférables, mais leur utilisation n'est pas toujours possible. Ce qui suit décrit une voie médiane : raisonner de façon semi-formelle à propos de la *correctness*.

L'approche sous-jacente consiste à **diviser tout le code à l'étude en de courtes parties** — d'une simple ligne, comme un appel de fonction, à un bloc d'au plus dix lignes — et débattre de leur *correctness*. Les arguments ont seulement besoin d'être suffisamment puissants pour convaincre l'avocat du diable qui te sert de *peer programmer*.

Une section devrait être choisie pour à chaque *endpoint*, **l'état du programme** (c'est à dire, [le compteur ordinal](https://fr.wikipedia.org/wiki/Compteur_ordinal) et les valeurs de tous les objets « vivants ») satisfasse une propriété clairement décrite, et que les fonctionnalités de cette section soient faciles à résumer en une unique tâche — cela rendra le raisonnement plus simple. Les propriétés de ces *endpoints* généralisent des concepts comme les **préconditions** et les **postconditions** pour les fonctions, ou les **invariants** pour les boucles et les classes. S'efforcer de rendre les sections aussi indépendantes les unes des autres simplifie le raisonnement et devient indispensable quand celles-ci doivent être modifiées.

Beaucoup de pratiques de codage bien connues (bien que sans doute moins bien suivies)  et considérées comme bonnes rendent le raisonnement plus facile. Par conséquent, rien que de commencer à raisonner sur ton code te mets dans un état d'esprit d'amélioration du style et de la structure. Sans surprise, la plupart des ces pratiques peuvent être vérifiées par des analyseurs de code statiques.

1.  Évite d'utiliser <code class="EnlighterJSRAW" data-enlighter-language="c">goto</code>, parce qu'il rend les sections fortement interdépendantes.
2.  Évite d'utiliser des variables globales mutables parce qu'elles rendent toutes les sections les utilisant dépendantes.
3.  Chaque variable devrait avoir **la plus petite portée possible**. Par exemple, un objet local devrait être déclaré juste avant son premier usage.
4.  Rendre les objets **immutables** chaque fois que c'est pertinent.
5.  Rend le code lisible **par l'espacement**, tant horizontal que vertical. Par exemple, en alignant les structures connexes et en utilisant une ligne vide pour séparer deux sections.
6.  Choisis des** noms descriptifs** (mais relativement courts) pour les objets, les types, les fonctions etc, afin de rendre le code auto-documenté.
7.  Si tu as besoin d'une section imbriquée, transforme-la en fonction.
8.  Rend tes fonctions courtes et focalisées sur une tâche unique. La **vieille limite des 24 lignes** s'applique toujours. Bien que la taille et la résolution des écrans aient changées, ce n'est pas le cas de la cognition humaine depuis les années 1960.
9.  Les fonctions devraient avoir **peu de paramètres** (quatre est une bonne limite). Cela ne réduit pas les données à communiquer aux fonctions : en regroupant des paramètres connexes dans un même objet, cela bénéficie aux invariants de l'objet et aide au raisonnement, par exemple sur leur cohérence ou leur uniformité.
10.  Plus généralement, chaque unité de code, du bloc à la bibliothèque, devrait avoir une **interface réduite** (NdT : [narrow interface](http://wiki.c2.com/?NarrowTheInterface)). Moins de communication réduit le raisonnement à faire. Cela signifie que les **getters** qui retournent l'état interne d'un objet sont un handicap — ne demande à un objet des informations sur lesquelles travailler. À la place, demande à l'objet de faire son travail avec les informations qu'il a déjà. En d'autres mots, **l'encapsulation** c'est — en tout et pour tout — **une histoire de *narrow interfaces***.
11.  Afin de préserver les invariants de classes, l'utilisation de **setters** devrait être découragée, puisque ils ont tendances à autoriser de casser les invariants gouvernant l'état de l'objet.

De même que raisonner à propos de sa *correctness*, débattre sur son code **en donne une meilleure compréhension**. Communique les idées que tu en dégage aux autres, pour le bénéfice de tout le monde.

## Mon mot à moi

J'ai trouvé ce chapitre particulièrement difficile à traduire, alors que, paradoxalement, les principes présentés sont simples et efficaces. Je pense notamment à la **[programmation par contrat](https://fr.wikipedia.org/wiki/Programmation_par_contrat)**. Un programmeur C++ expérimenté du nom de [lmghs](https://zestedesavoir.com/membres/voir/lmghs/) a d'ailleurs écrit une série de trois articles que je t'invite à aller lire (1) [Un peu de théorie](http://luchermitte.github.io/blog/2014/05/24/programmation-par-contrat-un-peu-de-theorie/), 2) [Les assertions](http://luchermitte.github.io/blog/2014/05/28/programmation-par-contrat-les-assertions/) et 3) [Snippets pour C++](http://luchermitte.github.io/blog/2015/09/17/programmation-par-contrat-snippets-pour-le-c-plus-plus/)). On retombe une fois de plus, pour moi, dans le *craftmanship* et l'idée de faire un code clair, concis, propre. Ça me rappelle aussi l'importance du chapitre sur [l'orthogonalité](http://www.artima.com/intv/dry3.html) du livre The Pragmatic Programmer.

Je sais bien que les getters et setters sont un sujet à débat parmi les développeurs, mais je suis de l'avis de [Yechiel](http://programmer.97things.oreilly.com/wiki/index.php/Yechiel_Kimchi). Les getters donnent un accès en lecture à des attributs et se justifient assez simplement dans la plupart des cas. Par contre, il faut **bien les nommer**. J'ai en tête les conventions en Java qui font commencer chaque getter par <code class="EnlighterJSRAW" data-enlighter-language="java">Get</code>, ce qui est inutilement redondant à mon goût. À l'inverse, j'aime bien les *properties* de C# puisqu'on accède à l'attribut simplement, sans fioriture.

Concernant les setters, c'est un peu plus « délicat », disons. Un premier problème est qu'ils sont souvent créé comme ça, en même temps que les getters, parce que ça fait bien. Mais ont-ils du sens ? C'est la première question à se poser : « **mon setter est-il justifié ?** » Si l'objet est immutable par définition, ça n'a pas de sens de modifier ses attributs. Un [exemple](http://www.yegor256.com/2014/09/16/getters-and-setters-are-evil.html) serait un animal. On ne modifie pas le poids d'un animal comme ça, avec un banal <code class="EnlighterJSRAW" data-enlighter-language="java">Set</code>. On peut à la limite utiliser une méthode <code class="EnlighterJSRAW" data-enlighter-language="java">JeTeDonneTropÀMangerSansQueTuFassesDuSport</code>, mais bon, pauvre animal.

Une autre question se pose à propos des **contrôles de validité**. Quid de l'utilisateur qui rentre une mauvaise valeur ? Si un objet pète à cause d'un set qui ne vérifie rien, c'est un mauvais design, point. J'aime bien [le post de gbdivers](https://openclassrooms.com/forum/sujet/liste-d-initialisation-et-conditions-setters#message-88693812), que je mets pour la postérité.

> Prenons un exemple concret : un véhicule (pour faire plaisir à notre koala préféré)
> 
> Tu vas peut être avoir un membre "réservoir d'essence", qui contient la quantité d'essence dans le réservoir.
> 
> Pour lorsque tu vas mettre de l'essence dans le réservoir, cela ne va pas simplement changer la variable membre, mais également changer le poids du véhicule, casser le moteur si tu mets la mauvaise essence, permettre de rouler plus vite si tu mets de l'essence avec additifs, etc. C'est la différence fondamentale entre "modifier une variable membre" (setter) et rendre un service en conception objet.
> 
> Bien sur, si tu n'as pas toutes ces fonctionnalités à gérer dans ta classe, tu vas peut être au final te retrouver avec un code proche de ton "setNb", qui ne fait que vérifier que tu ne dépasses pas la capacité du réservoir et qui modifie simplement ta variable membre.
> 
> Il est probable que l'on préférera dans ce cas un fonction "addEssence" plutôt que "setEssence", pour éviter d'utiliser cette fonction à mauvais escient. Par exemple, on pourrait être tenté de calculer la distance parcouru par le véhicule, multiplier par la consommation kilométrique et faire un setEssence pour modifier la quantité d'essence. Et là, très clairement, on pense le véhicule en termes d'ensemble de variables et non en tant que services rendus.
> 
> Le problème au final n'est pas les setter/getter. Ce sont des fonctions qui réduisent la qualité du code (couplage plus fort entre interface et implémentation des classes), mais cela peut être un choix volontaire de la part du développeur. Le problème est surtout la "mauvaise" façon de concevoir ses classes, l'utilisation de accesseurs qui en découle, et le fait que faire cela par défaut, sans comprendre les conséquences sur la qualité du code.

En fait, dans le cas où l'on n'a pas vraiment de vérification à faire sur un objet, type `Coordonnées`, alors autant ne pas se casser la tête et passer les attributs en public ou type *properties* en C#, par exemple.

**TL;DR** : attention aux setters et oui à la réflexion et la raison quand on code !
