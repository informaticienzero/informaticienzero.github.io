---
layout: default
title: "Langage spécifique au domaine — 97 choses qu’un programmeur doit savoir"
date: 2017-08-03 11:32:12
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Langage spécifique au domaine](http://programmer.97things.oreilly.com/wiki/index.php/Domain-Specific_Languages) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/)me le permet. Il a été écrit à l’origine par [Michael Hunger](http://programmer.97things.oreilly.com/wiki/index.php/Michael_Hunger "Michael Hunger").

<!--excerpt-->

Chaque fois que tu écoutes une discussion entre experts d'un quelconque domaine, que ce soit des joueurs d'échec, des enseignants de maternelles, ou des agents d'assurance, tu remarqueras que leur vocabulaire est assez différent du langage de tous les jours. C'est ce à quoi servent les langages spécifiques au domaine (NdT : tiré de l'anglais *domain-specific languages*, abrégé en DSLs) : un domaine spécifique a un vocabulaire spécialisé pour décrire des choses particulières au-dit domaine.

Dans le monde du logiciel, les DSLs parlent d'expressions exécutables, dans un langage spécifique à un domaine, avec un vocabulaire limité et dont la grammaire est lisible, compréhensible et — heureusement — écrivable — par des experts du domaine. Les DSLs utilisés par les développeurs ou les scientifiques sont là depuis un long moment. Par exemple, les « petits langages » Unix, qu'on trouve dans les fichiers de configurations, ainsi que les langages créés avec le pouvoir des macros LISP sont parmis les plus vieux exemples.

Les DSLs sont fréquemment classés comme **internes** ou **externes**.

- **Les DSLs internes** sont écrits dans un langage de programmation quelconque dont la syntaxe a été tordue pour ressembler à un langage naturel. C'est plus facile avec des langages qui offrent plus de sucre syntaxique et de possibilités de formatage (comme Ruby ou Scala) qu'avec ceux qui ne le font pas (Java). La majorité des DSLs internes wrappent des APIs existantes, des bibliothèque ou du code business et fournissent ainsi un accès plus facile et moins pénible à la fonctionnalité. Ils s'éxécutent simplement en les lançant. En fonction de l'implémentation et du domaine, ils sont utilisés pour construires des structures de données, définir des dépendances, lancer des processus ou des tâches, communiquer avec d'autres systèmes, ou valider des entrées utilisateur. La syntaxe d'un DSL interne est contrainte par le langage hôte (NdT : de l'anglais *host language*). Il y a de nombreux patterns — expression builder, chaînage de méthodes, annotation — qui peuvent t'aider à adapter le langage hôte à ton DSL interne. D'ailleurs, si le langage hôte ne requiert par de recompilation, un DSL interne peut être développé rapidement, avec l'appui d'un expert domaine.

- **Les DSLs externes **sont des expressions textuelles ou graphiques du langage textual — bien que les DSLs textuels semblent de plus en plus répandus par rapport aux graphiques. Des expressions textuelles peuvent être analysées par une tool chain incluant un lexer, un parser, un model transformer, des générateurs et encore d'autres types de post-processing. Les DSLs externes sont principalement lus par des modèles internes, permettant un traitement ultérieur. Il est utile de définir une grammaire, qui va ainsi fournir le point de départ pour générer des parties de la tool chain (éditeur, visualisateur, générateur de parseur, etc). Pour des DSLs simples, un parseur fait main peut être suffisant — en utilisant des expressions régulières, par exemple. Par contre, ils peuvent devenir lourds si on leur en demand trop, auquel cas cela a du sens de regarder des outils spécifiquement conçus pour travailler avec des DSLs et des grammaires — par exemple, openArchitectureWare, ANTlr, SableCC, AndroMDA. L'utilisation d'un dérivé de XML par un DSL externe est fréquente, bien que la lisibilité puisse être un problème, en particulier pour ceux non versés dans la technique.

Tu dois toujours prendre en compte le public de ton DSL. Sont-ils des développeurs, des managers, des clients business, les utilisateurs finaux ? Tu dois adapter le niveau technique du langage, les outils disponibles, l'aide à la syntaxe (intellisense), la visualisation et la représentation en fonction de l'audience. *By hiding technical details, DSLs can empower users by giving them the ability to adapt systems to their needs without requiring the help of developers. It can also speed up development because of the potential distribution of work after the initial language framework is in place. The language can be evolved gradually. There are also different migration paths for existing expressions and grammars available *(NdT : je n'arrive pas à traduire).

## Mon mot à moi

Ce chapitre aura été extrêmement dur à traduire parce que, sans recherche complémentaire, je n'aurai pas compris de quoi parle l'auteur. L'[article Wikipédia](https://fr.wikipedia.org/wiki/Langage_d%C3%A9di%C3%A9) est d'une grande aide pour ça.

Par exemple, la différence entre un DSL interne et externe devient plus claire. Le premier sera utilisé **au sein d'un code source** écrit dans un langage hôte (type Haskell, Lisp, etc) alors que le deuxième permet de **créer des programmes indépendants** de tout autre langage de programmation.

On peut voir **SQL comme un DSL** puisqu'il est **cantonné à la manipulation de bases de données** et le permet plus facilement que s'il fallait appeler une longue liste de routines en C ou en C++. Un autre exemple que j'avais vu : **l'utilisation de Lua au sein d'un programme C++** qui implémentait un système d'Entities, Components, Systems et qui, par l'utilisation d'un langage simple comme Lua, permettait à un utilisateur non versé dans le C++ d'ajouter des personnages ou des caractéristiques à des personnages. Ou encore **LINQ**, le langage de requête utilisable au sein d'un programme C#.

En DSL externe, citons [**Cucumber**](https://en.wikipedia.org/wiki/Cucumber_(software)), un programme qui fournit un langage permettant de tester d'autres logiciels en utilisant un langage appelé Gherkin.

J'encourage chaque lecteur à lire l'article Wikipédia qui est plus clair et, surtout, qui contient des exemples qui aident à bien saisir la notion.