---
layout: default
title: "Se demander « Que ferait l'utilisateur ? » — 97 choses qu’un programmeur doit savoir"
date: 2016-05-16 21:36:06
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Se demander « Que ferait l'utilisateur ? »](http://programmer.97things.oreilly.com/wiki/index.php/Ask_%22What_Would_the_User_Do%3F%22_%28You_Are_not_the_User%29) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Giles Colborne](http://programmer.97things.oreilly.com/wiki/index.php/Giles_Colborne).

<!--excerpt-->

Nous avons tendance à **présumer que les autres pensent comme nous**. Mais ce n'est pas le cas. On appelle ça l'effet de faux consensus. Quand des gens pensent ou agissent différemment de nous, inconsciemment nous sommes tentés de les classer comme déficients, d'une certaine façon.

Cet effet explique pourquoi les programmeurs ont tant de mal à se mettre à la place des utilisateurs. Ces derniers ne pensent pas comme les premiers. Déjà, ils passent beaucoup moins de temps derrière un ordinateur. Ils ne connaissant pas, et bien souvent ne s'intéressent pas, au fonctionnement d'un PC. Cela signifie qu'ils ne peuvent pas se reposer sur toutes les techniques de résolution de problèmes si familière aux développeurs. Ils ne reconnaissent pas les patterns et les repères avec lesquels les programmeurs ont l'habitude de travailler sur, autour et à travers une interface.

Le meilleur moyen de comprendre comment pense un utilisateur est encore d'**en regarder un**. Demande à un utilisateur de faire une tâche qui utilise un morceau de logiciel similaire à celui que tu développes. Sois bien sûr que la tâche à réaliser est bien réelle : « Additionne une colonne de nombres » c'est OK ; « Calcule tes dépenses du mois passé » c'est mieux. Évite les tâches trop spécifiques : « Peux-tu sélectionner ces cellules du tableau et entrer une formule SUM en-dessous ? » - il y a un indice dans la question. Fais parler l'utilisateur pendant qu'il fait ce que tu lui a demandé. Mais ne l'interromps pas et ne l'aide pas. Plutôt, continue de te demander « Pourquoi est-ce qu'il fait ça ? » et « Pourquoi est-ce qu'elle ne fait pas ça ? »

[![Le programme échoue au test de la ménagère.](http://www.commitstrip.com/wp-content/uploads/2014/01/Strips-Le-test-de-ta-m%C3%A8re-650-final.jpg)](http://www.commitstrip.com/wp-content/uploads/2014/01/Strips-Le-test-de-ta-m%C3%A8re-650-final.jpg)

La première chose qu'on remarque, c'est que les utilisateurs ont tendance à **regrouper les actions similaires**. Ils tentent de remplir les tâches dans le même ordre et font les erreurs aux mêmes endroits. Il faut établir le design en se basant sur ce comportement central. C'est différent des réunions de design où l'on entend bien souvent « Et si jamais l'utilisateur veut ... ? » Cela mène à des fonctionnalités élaborées et de la confusion par rapport à ce qu'ils veulent. Observer les utilisateurs élimine cette confusion.

Tu vas voir les utilisateurs se retrouvés coincés. Quand tu es coincé, tu regardes autours. Quand ça arrive aux utilisateurs, leur attention se réduit. Il leur devient plus difficile de trouver la solution ailleurs sur l'écran. C'est une des raisons pour lesquelles un texte d'aide n'est qu'une mauvaise solution à un mauvais design d'interface. Si tu es obligé de mettre des instructions ou un texte d'aide, sois sûr de **le placer juste à côté de l'endroit problématique**.

Les utilisateurs ont tendance à se débrouiller. Ils trouveront un moyen de faire ce qu'ils veulent et s'en contenteront, peu importe à quel point ça peut être alambiqué. Il est donc préférable de fournir un moyen évident de faire les choses plutôt que deux ou trois raccourcis. Tu verra qu'il y a aussi un fossé entre ce que les utilisateurs disent vouloir et ce qu'ils font. Le meilleurs moyen de comprendre ce qu'ils veulent est donc de **les observer**. Regarder comment procède les utilisateurs pendant une heure est plus instructif que de passer une journée à deviner ce qu'ils veulent.

## Mon mot à moi

Je n'en ai pas, je n'ai jamais fais d'interface utilisateur. Mais je prends note.
