---
layout: default
title: "Version finale du tutoriel C"
date: 2016-12-26 17:22:12
permalink: /:title/
---
Aujourd'hui est un grand jour : [le tutoriel sur le C](https://zestedesavoir.com/tutoriels/755/le-langage-c-1/), auquel j'ai contribué, a été mis à jour ! Bien entendu, il restera toujours des petites corrections ou améliorations, mais **la version finale est publiée** !

<!--excerpt-->

Au programme de la mise à jour, pas moins de 11 chapitres.

* La représentation des types, pour expliquer comment la norme C précise (ou pas) le stockage des entiers, des flottants et des pointeurs, ainsi que la famille `memset`.
* Les limites des types, avec notamment la gestion du dépassement de capacité.
* La manipulations des bits, avec notamment les masques, les champs et les flags.
* Les jeux de caractères et les encodages.
* Les énumérations.
* Les unions.
* Les définitions de types, a.k.a `typedef`.
* Les pointeurs de fonctions (chose puissante mais dont je suis bien content que ce soit encapsulé dans les langages de plus haut niveau).
* Les fonctions à nombre variable d'arguments (mais si, les trucs dans les prototypes de `printf` ou les [templates C++](https://en.wikipedia.org/wiki/Variadic_template)).
* Un T.P proposant de créer un allocateur statique de mémoire, qui consiste à imiter `malloc` et `free`, mais de façon statique (aller taper dans des appels systèmes pour faire ça dynamiquement est un peu plus complexe).
* Un chapitre d'index pour trouver rapidement les informations recherchées.

Bon, il reste un tout petit peu de travail, notamment sur deux anciens chapitres, présentant respectivement quelques bibliothèques célèbres en plus de faire le tour de la bibliothèque standard, ainsi que sur C99 et C11. On ne sait pas encore si on les intégrera ou non. Mais vraiment, **quel plaisir de voir 5 années d'efforts et de patience porter leurs fruits**. J'espère vraiment que ce tutoriel plaira.

## Lance-toi

J'avais écris, à l'occasion de sa publication début 2016, [une rétrospective](consécration-d'années-d'écriture-et-rétrospective). **J'encourage de nouveau tout ceux qui rêvent de partager leurs connaissances de se lancer**. Sur ZdS certes, mais n'importe où ailleurs. L'exercice demande de l'abnégation et de la persévérance, mais il est très formateur. Et partager ses connaissances, peu importe le domaine, est un geste magnifique. Alors fonce !