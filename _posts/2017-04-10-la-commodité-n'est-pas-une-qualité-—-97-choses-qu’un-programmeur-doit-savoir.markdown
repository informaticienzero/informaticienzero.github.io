---
layout: default
title: "La commodité n'est pas une qualité — 97 choses qu’un programmeur doit savoir"
date: 2017-04-10 07:00:43
permalink: /:title/
---
Aujourd'hui, c’est le chapitre [La commodité n'est pas une qualité](http://programmer.97things.oreilly.com/wiki/index.php/Convenience_Is_not_an_-ility) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Gregor Hohpe](http://programmer.97things.oreilly.com/wiki/index.php/Gregor_Hohpe "Gregor Hohpe").

<!--excerpt-->

Beaucoup a déjà été dit à propos de l'importance et des défis d'un bon design d'API. C'est difficile de faire bien du premier coup et encore plus difficile de changer ultérieurement. Un peu comme élever des enfants. La plupart des programmeurs expérimentés ont appris qu'une bonne API suit un **niveau constant d'abstraction**, est **cohérente**, **symétrique** et est le **vocabulaire d'un langage expressif**. Hélas, être au courant de ces principes conducteurs ne veut pas forcément dire qu'on les appliquera bien. Manger des bonbons n'est pas bon pour toi.

Au lieu de faire un sermon, je vais choisir une « stratégie » de design d'API bien particulière, celle que j'ai vu, encore et encore : l'argument de commodité. Ça commence typiquement avec une de ces idées :

- Je ne veux pas que les autres classes aient besoin de faire deux appels séparés pour faire cette unique chose.
- Pourquoi devrais-je faire une autre méthode si c'est quasiment la même que celle-ci ? Je n'ai qu'à faire un `switch`.
- Regarde, c'est très simple : si le deuxième paramètre string termine avec ".txt", la méthode déduit automatiquement que le premier paramètre est un nom de fichier, donc je n'ai pas besoin de deux méthodes.

Bien que partant d'une bonne intention, de tels arguments sont susceptibles de diminuer la lisibilité d'un code faisant appel à cette API. Un appel de méthode comme `parser.processNodes(text, false);`  est **pratiquement incompréhensible sans connaître l'implémentation**, ou tout du moins sans regarder la documentation. Cette méthode a certainement été conçue pour la convenance du concepteur et non pour celle de l'utilisateur — « Je ne veux pas que l'utilisateur ait à faire deux appels » devient « Je ne voulais pas coder deux méthodes différentes. » Il n'y a rien de mal à la commodité, à la convenance, si c'est dans le but d'être un antidote à la fatigue, à la lourdeur ou à la maladresse. Cependant, si l'on s'y penche un peu plus, l'antidote à ces choses est l'efficacité, la constance, et l'élégance, par forcément la commodité. Les API sont sensés cacher la complexité sous-jacente, donc on peut s'attendre à ce qu'**un bon design d'API demande des efforts**. Une grosse méthode serait certainement bien plus commode à écrire qu'un lot bien pensé d'opérations, mais serait-ce plus simple à utiliser ?

La métaphore comparant une API à une langue peut nous aider à prendre de meilleures décisions concernant son design. Une API doit fournir un langage expressif, qui donne à la couche suivante un vocabulaire suffisant pour poser des questions et y donner des réponses utiles. Cela ne veut pas dire pour autant qu'il faut fournir exactement une méthode, ou verbe, pour chaque question qui pourrait être posée. **Un vocabulaire varié permet d'exprimer des subtilités**. Par exemple, on préfère dire `run`  plutôt que `walk(true)` , même s'il s'agit quasiment de la même opération, exécutée à des vitesses différentes. Un vocabulaire d'API cohérent et bien pensé rend un code expressif et facile à comprendre dans la couche supérieure. Plus important, un vocabulaire composable autorise les autres programmeurs à utiliser l'API d'une manière que tu n'avais peut-être pas anticipé — une grande commodité pour les utilisateurs de l'API, en effet !

La prochaine fois que tu es tenté de regrouper plusieurs choses en une seule méthode d'API, rappelle toi que l'anglais (NdT : le français non plus) n'a pas de mot pour « Range ta chambre, tais toi et fais tes devoirs », bien que ça puisse être très commode vu la fréquence de la requête (NdT : trooool).

## Mon mot à moi

J'avoue que je n'ai jamais eu à créer vraiment d'API, tout au plus quelques requêtes SQL encapsulées dans des fonctions. Par contre, j'ai eu l'occasion d'en utiliser, notamment dans le cadre d'ajouts de tests unitaires à un projet qui n'en avait pas. Et ça n'est pas facile à faire, quand une fonction sensé encapsuler une simple requête SQL fait beaucoup trop de choses. Exactement le travers décrit par Gregor. Le programmeur responsable de l'écriture de cette fonction a d'abord pensé à lui avant de penser à l'utilisateur. En même temps, pour ne pas mettre de tests unitaires dans le projet, il faut vraiment être égoïste et ne pas penser à ceux qui viennent après. Mais ça, les mauvais programmeurs, c'est une autre histoire.