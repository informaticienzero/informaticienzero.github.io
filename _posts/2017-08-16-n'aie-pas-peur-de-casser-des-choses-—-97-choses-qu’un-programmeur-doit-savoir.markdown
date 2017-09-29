---
layout: default
title: "N'aie pas peur de casser des choses — 97 choses qu’un programmeur doit savoir"
date: 2017-08-16 11:54:18
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [N'aie pas peur de casser des choses](http://programmer.97things.oreilly.com/wiki/index.php/Don%27t_Be_Afraid_to_Break_Things) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Mike Lewis](http://programmer.97things.oreilly.com/wiki/index.php/Mike_Lewis "Mike Lewis").

<!--excerpt-->

Tout le monde ayant une expérience du monde professionnel a déjà travaillé sur un projet où la base de code était, au mieux, précaire. Le système est pauvrement découpé, et changer un élément finit toujours par casser une autre feature sans rapport. Chaque fois qu'un module est ajouté, l'objectif du codeur est de ne changer que le moins possible, et on retient sa respiration à chaque release. C'est l'équivalent logiciel de jouer au [Jenga](https://fr.wikipedia.org/wiki/Jenga) avec des barres [I-beams](https://en.wikipedia.org/wiki/I-beam) dans un gratte-ciel, et c'est destiné au désastre.

La raison pour laquelle faire des changements est si éprouvant pour les nerfs, c'est que le système est malade. Il a besoin d'un docteur, sinon ses conditions ne vont qu'empirer. Tu sais déjà ce qui ne va pas dans le système, mais tu as peur de casser des œufs pour faire ton omelette. Un chirurgien qualifié sait qu'il faut des coupures afin d'opérer, mais il sait également qu'elles sont temporaires et vont guérir. Le résultat final vaut la peine de supporter la douleur initiale, et le patient guérira et finira dans un meilleur état qu'avant l'opération.

N'aie pas peur de ton code. On s'en fout si quelque chose est temporairement cassé, le temps que tu replaces les morceaux. C'est une peur paralysante du changement qui a mené ton projet dans cet état. Le temps investi à refactorer va payer de multiples fois au cours de la vie du projet. L'un des bénéfices de traiter avec un système malade est que toi et l'équipe avez une expérience de comment le système **devrait** fonctionner. Applique ce savoir plutôt que de le ressentir. Travailler sur un système que tu détestes, ça n'est pas la façon dont on devrait passer son temps.

Redéfinis les interfaces internes, restructure les modules, refactorise du code copié-collé et simplifie le design en réduisant les dépendances. Tu peux grandement réduire la complexité du code en éliminant les *corner cases*, qui sont souvent la conséquence de features improprement couplées. Lentement, fais la transition de l'ancienne structure vers la nouvelle, en testant tout le long. Tenter de faire un gros refactoring en un seul coup va te causer suffisamment de problèmes pour être tenté d'abandonner en chemin.

Sois le chirurgien qui n'a pas peur de couper les parties malades pour laisser place à la guérison. Cette attitude est contagieuse et inspirera les autres à se mettre au nettoyage tant repoussé. Garde une liste d'hygiène des tâches que l'équipe juge dignes d'intérêt pour le bien du projet. Convainc le management que, même si ces tâches ne produisent pas de résultat visible, elles réduiront les dépenses et accélèreront les releases futures. Ne t'arrête jamais de t'occuper de la santé générale du code.

## Mon mot à moi

Encore un article en plein dans la mouvance du software craftsmanship. On veut faire les choses certes, mais les faire bien. D'ailleurs, Oncle Bob décrit ce processus d'amélioration par petites touches comme d'inspiration scout : laisser l'endroit dans un meilleur état que l'on ne l'a trouvé. Cela passe, par exemple, par de la factorisation de code redondant, du renommage de variables ou de fonctions mal nommées, par de la simplification d'une condition complexe. Par petite touche, la qualité progresse.

Si je suis entièrement d'accord avec cette théorie, et si j'essaye de l'appliquer au travail, le fait est que c'est très compliqué. Malgré des refactorings bienvenus, les collègues ne sont pas emballés puisque « le système a toujours été comme ça ». Alors forcément, la volonté s'affaiblit et on a de moins en moins envie de travailler dessus.

L'autre blocage est le management, qui, s'il est stupide, ne voit pas l'intérêt de ces tâches de soin, et continue donc à charger les développeurs avec de nouvelles fonctionnalités ou des corrections de bugs, bugs qui ont échappé à la phase de test justement parce que le code a besoin de nettoyage mais que personne n'autorise les développeurs à le faire. Que veux-tu, si le management était intelligent, ça se saurait depuis longtemps.