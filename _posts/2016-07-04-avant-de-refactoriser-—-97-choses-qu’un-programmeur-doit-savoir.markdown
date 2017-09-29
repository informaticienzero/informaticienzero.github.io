---
layout: default
title: "Avant de refactoriser — 97 choses qu’un programmeur doit savoir"
date: 2016-07-04 19:11:39
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Avant de refactoriser](http://programmer.97things.oreilly.com/wiki/index.php/Before_You_Refactor) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Rajith Attapattu](http://programmer.97things.oreilly.com/wiki/index.php/Rajith_Attapattu).

<!--excerpt-->

Arrivé un certain moment, tout programmeur doit refactoriser du code existant. Mais avant que tu ne le fasses, prends, s'il-te-plaît, du temps pour réfléchir aux points suivants, qui peuvent te faire gagner, à toi et aux autres, du temps et t'épargner des douleurs.

- **La meilleure approche pour restructurer consister à faire le point sur la base de code existante et les tests écrits pour ce code**. Cela t'aidera à comprendre les forces et faiblesses du code tel qu'il est, pour être sûr d'en conserver les points forts tout en éliminant les points faibles. On pense tous pouvoir mieux faire que le système en place ... jusqu'à ce qu'on se retrouve avec quelque chose qui n'est pas mieux — voire pire — que l'ancien modèle parce qu'on a échoué d'en apprendre les erreurs.

- **Fuis la tentation de tout réécrire**. Il est mieux de réutiliser le plus possible de code. Peu importe la mocheté du code, il a déjà été testé, revu, etc. Jeter du vieux code — particulièrement s'il était en production — signifie que tu jettes des mois, voire des années, de code testé, endurci (NdT : *battle-hardened code*) qui a pu avoir des *workarounds* et des correctifs dont tu n'es pas au courant. Si tu ne prends pas ça en compte, le nouveau code que tu écris pourrait finir par présenter les mêmes bugs mystérieux pourtant corrigés dans l'ancien code. Cela ne va entraîner qu'une vaste perte de temps, d'efforts et de connaissances acquis au cours des années.

- **Pleins de changements graduels sont mieux qu'un changement massif**. Les changements graduels te permettent de jauger l'impact sur le système plus facilement grâce à des retours, comme les tests. Ça n'est pas plaisant du tout de voir une centaine de tests échouer après avoir effectué un changement. Ça peut mener à de la frustration et de la pression qui peuvent conduire à prendre de mauvaises décisions. Deux ou trois tests qui échouent sont plus faciles à gérer et permettent une approche plus gérable.

- **Après chaque itération, il est important de s'assurer que les tests existants passent**. Ajoute de nouveaux tests si ceux existants ne sont pas suffisants pour couvrir les changements que tu as fais. Ne jette pas les tests de l'ancien code sans bien y réfléchir. En apparence, certains tests peuvent ne pas s'appliquer à ton nouveau design, mais il serait judicieux de creuser pour comprendre les raisons de l'ajout de ce test.

- **Les préférences personnelles et l'égo ne devraient pas intervenir**. Si quelque chose n'est pas cassé, pourquoi le réparer ? Le fait que le style ou la structure du code ne correspond pas à tes préférences personnelles n'est pas une raison valable pour restructurer. Penser que tu puisses faire un meilleur travail que le programmeur précédent non plus.

- **Une nouvelle technologie n'est pas une raison suffisante à une refactorisation**. L'une des pires raisons d'une refactorisation est que le code est loin derrière toutes les technos super cool qu'on a aujourd'hui et qu'on pense qu'un nouveau langage ou framework puisse faire les choses d'une façon bien plus élégante. À moins qu'une analyse coûts–bénéfices ne montre qu'un nouveau langage ou framework résultera en une amélioration significative en terme de fonctionnalité, maintenabilité ou productivité, il vaut mieux laisser les choses telles qu'elles sont.

- **Souviens-toi que l'erreur est humaine**. Restructurer ne garantie pas que le nouveau code sera meilleur – ou même aussi bon – que l'ancien. J'ai déjà vu et participé à des échecs sévères de restructurations. Ce n'était pas intelligent, mais c'était humain.

## Mon mot à moi

Au travail, dans mon équipe, nous maintenons une base de code veille de plusieurs années. Tous ces principes s'appliquent donc dans mon quotidien. Et j'ai déjà fais le contraire des principes préconisés ici, notamment celui de penser que je peux réécrire le code d'une façon plus efficace et plus élégante que l'existant. Heureusement que le TechLead était là pour me recadrer. C'est rare, mais là **je suis vraiment d'accord avec tous les principes**.