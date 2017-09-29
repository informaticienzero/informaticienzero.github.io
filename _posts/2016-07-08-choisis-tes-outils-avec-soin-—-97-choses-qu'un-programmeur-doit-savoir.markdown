---
layout: default
title: "Choisis tes outils avec soin — 97 choses qu'un programmeur doit savoir"
date: 2016-07-08 11:30:24
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Choisis tes outils avec soin](http://programmer.97things.oreilly.com/wiki/index.php/Choose_Your_Tools_with_Care) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Giovanni Asproni](http://programmer.97things.oreilly.com/wiki/index.php/Giovanni_Asproni).

<!--excerpt-->

Les applications modernes ne sont que très rarement construites à partir de rien. Au contraire, elles sont souvent un assemblage d'outils existants — composants, bibliothèques, frameworks — pour un certains nombre de bonnes raisons.

* Les applications grandissent en taille, en complexité et en sophistication, tandis que le temps disponible pour développer se réduit au fur et à mesure. C'est faire un meilleur usage du temps et de l'intelligence des développeurs que de **plus se concentrer sur du code business et moins sur du code d'infrastructure**.

* Les composants et frameworks largement utilisés sont beaucoup **moins susceptibles d'avoir des bugs** que ceux faits maison.

* Il y a une grande quantité de logiciels de haute qualité **disponibles gratuitement** sur le web, ce qui signifie des coûts de développements réduits et une probabilité augmentée de trouver des développeurs avec l'intérêt et l'expertise demandés.

* La création et la maintenance logicielle demande un **travail humain intensif**, donc acheter peut revenir moins cher que faire soi-même.

Cependant, choisir le bon mix d'outils pour son application peut devenir une affaire délicate demandant une profonde réflexion. En fait, quand on fait un choix, il faut garder plusieurs choses à l'esprit.

* Les différents outils peuvent se baser sur des **suppositions différentes quant à leur contexte** — par exemple, l'infrastructure environnante, le modèle de contrôle, le modèle de données, les protocoles de communication, etc — ce qui peut mener à une inadéquation entre l'application et les outils. Une telle inadéquation mène à des hacks et des solutions de contournement qui rendent le code plus complexe que nécessaire.

* Les différents outils peuvent avoir des **cycles de vie différents** et en mettre un à jour peut devenir extrêmement difficile et consommateur de temps parce que de nouvelles fonctionnalités, un changement de design, ou même une simple correction de bug peut provoquer des incompatibilités avec les autres outils. Plus il y a d'outils, plus le problème empire.

* Certains outils demandent **un peu de configuration**, souvent par le moyen d'un ou plusieurs fichiers XML, qui peuvent grandir de façon incontrôlée très rapidement. Au final, on peut se demander si l'application n'a pas été écrite en XML avec quelques lignes de code d'un quelconque langage de programmation. Trop complexe, cela rend l'application plus difficile à maintenir et faire évoluer.

* L'enfermement logiciel (NdT : *vendor lock-in*) intervient quand un code qui dépend beaucoup de détails logiciels ou matériels des produits d'un vendeur finit par **être contraints par ces produits** sur différents aspects : maintenance, performances, abilité à évoluer, prix, etc.

* Si tu comptes utiliser des logiciels gratuits, attends-toi peut-être à ce qu'il ne soit pas gratuit après tout. Tu seras peut-être obligé d'**acheter un support commercial**, qui sera tout sauf donné.

* **Les licences comptent**, même pour des logiciels gratuits. Par exemple, dans certains compagnies, ce n'est pas acceptable d'utiliser des logiciels sous licence GNU de part sa nature virale — exemple : un logiciel développé avec doit distribuer le code source.

Ma stratégie personnelle pour atténuer ces problèmes est de **commencer petit, par les outils absolument nécessaires**. D'habitude, on se concentre pour éliminer le besoins de se plonger dans de la programmation bas-niveau d'infrastructure (et des problèmes liés), par exemple en utilisant des *middlewares* au lieu de sockets bruts pour des applications distribuées. Et seulement là on peut ajouter, si nécessaire. Je fais également mon possible pour isoler les outils externes de mes objets métiers par utilisation d'interfaces et de *layering*, afin de diminuer les efforts à fournir pour changer l'outil si besoin. Un effet positif de cette approche est que, généralement, je finis avec une application plus petite, qui utilise moins d'outils externes qu'à la base.

## Mon mot à moi

Pas grand chose à dire, je n'ai pas vraiment d'expérience dans le domaine. Mais clairement, pour le voir parfois au travail, il vaut mieux sacrifier un peu de temps et d'argent pour s'approprier un outil efficace, qui nous permettrait de nous concentrer sur le code métier, plutôt que de mal redévelopper maison. Je pense notamment à la gestion des accès. Peut-être que je devrais leur envoyer cet article ?