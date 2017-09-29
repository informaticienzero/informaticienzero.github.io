---
layout: default
title: "Ne commente que ce que le code ne dit pas — 97 choses qu’un programmeur doit savoir"
date: 2016-11-30 11:45:28
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Ne commente que ce que le code ne dit pas](http://programmer.97things.oreilly.com/wiki/index.php/Comment_Only_What_the_Code_Cannot_Say) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Kevlin Henney](http://programmer.97things.oreilly.com/wiki/index.php/Kevlin_Henney).

<!--excerpt-->

La différence entre la théorie et la pratique est plus grande en pratique qu'en théorie — une observation qui s'applique bien aux commentaires. En théorie, l'idée de commenter son code semble être bonne : on offre des détails au lecteur, une explication de ce qui se passe. Quoi de plus utile que d'être utile ? En pratique, cependant, les commentaires deviennent souvent un fléau. Comme toute forme d'écriture, **il est une compétence à l'écriture de bons commentaires**. Une grande partie de cette compétence consiste à savoir quand ne pas en écrire.

Quand le code est mal-formé (NdT : en anglais *ill-formed*), le compilateur, l'interpréteur et les autres outils vont sans aucun doute objecter. Si le code est, d'une certaine manière, fonctionnellement incorrect, les revues, les analyseurs statiques, les tests et son utilisation quotidienne dans un environnement de production vont éliminer la plupart des bugs. Mais les commentaires ? Dans [The *Elements of Programming Style*](https://en.wikipedia.org/wiki/The_Elements_of_Programming_Style), Kernighan et Plauger notent « qu'**un mauvais commentaire a une valeur nulle voire négative**. » Et pourtant, de tels commentaires jonchent la base de code et survivent d'une façon impossible à imaginer pour des erreurs de code. Ils sont une source constante de distraction et de désinformation, une entrave subtile mais constante au raisonnement d'un programmeur.

Que dire des commentaires qui ne sont pas techniquement faux, mais qui n'ajoutent aucune valeur au code ? **Ils ne sont que bruit**. Les commentaires qui ne sont font que répéter le code tels des perroquets n'offrent rien de plus au lecteur — dire quelque chose d'abord dans le code puis en langage naturel ne le rend pas plus vraie ou plus réel. Le code commenté n'est pas exécutable, donc il n'a aucune utilisé tant pour le lecteur qu'à l'exécution. Également, il devient vicié très rapidement. Tant le code commenté que les commentaires liés à la version du code tentent de répondre à des problèmes de versioning et de qualité. Ces questions ont déjà leurs réponses (d'une façon tellement plus efficace) avec les **outils de gestion de versions**.

Une prévalence de commentaires bruyants et incorrects dans une base de code encourage les programmeurs à ignorer tous les commentaires, soit en les sautant, soit en prenant des mesures concrètes pour les masquer. Les programmeurs sont pleins de ressources et **feront tout pour se débarrasser de ce qu'ils pensent être dommageable** : replier les commentaires, changer la coloration syntaxique pour qu'ils aient la même couleur que le fond, créer un script pour filtrer les commentaires. Pour préserver sa base de code d'un tel gâchis d'ingéniosité et pour réduire le risque de manquer les commentaires de qualité, il faut qu'ils soient traités comme s'ils étaient du code. Chaque commentaire devrait ajouter de la valeur, sinon ce n'est que déchets qui doivent être supprimés ou réécrits.

Qu'est-ce donc que la valeur ? Les commentaires doivent **dire ce que le code ne dit pas et ne peut dire**. Un commentaire expliquant ce que le code devrait dire est une invitation à changer la structure du code, ou les conventions de codage, de manière à ce que le code parle de lui-même. Au lieu de compenser des mauvais noms de méthodes ou de classes, change-les. Plutôt que de commenter des sections de code dans de longues fonctions, extrais-en des fonctions plus petites dont les noms traduisent bien l'intention originale. Essaye de t'exprimer autant que possible par le code. Les différences entre ce que tu peux exprimer par le code et tout ce que tu voudrais dire font de bons candidats à des commentaires utiles. Commente **ce que le code ne peut pas dire**, pas simplement **ce qu'il ne dit pas**.

## Mon mot à moi

La suite toute logique de [l'article précédent](un-commentaire-à-propos-des-commentaires-—-97-choses-qu’un-programmeur-doit-savoir). Bien que de savoir quand un commentaire est utile et quand il devient du bruit ne soit pas aisé, c'est un exercice que je m'efforce d'accomplir dans mon travail, malgré une base de code vieille de plus de 10 ans et qui contient souvent des commentaires inutiles puisque simple redite du code.

{% highlight csharp %}
// Settings for X
Rules.Add("X_NAME");
Rules.Add("X_CONFIG_FILE");
{% endhighlight %}

Alors je m'efforce de faire les choses proprement et d'améliorer le code petit à petit, chaque fois que je peux. Eh oui, c'est ça aussi le travail d'un développeur !