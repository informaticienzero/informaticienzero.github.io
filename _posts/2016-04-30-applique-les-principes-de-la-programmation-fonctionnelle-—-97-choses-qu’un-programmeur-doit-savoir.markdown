---
layout: default
title: "Applique les principes de la programmation fonctionnelle — 97 choses qu’un programmeur doit savoir"
date: 2016-04-30 13:02:20
permalink: /:title/
---
Aujourd'hui, c'est le chapitre [Applique les principes de la programmation fonctionnelle](http://programmer.97things.oreilly.com/wiki/index.php/Apply_Functional_Programming_Principles) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l'origine par [Edward Garson](http://programmer.97things.oreilly.com/wiki/index.php/Edward_Garson).

<!--excerpt-->

La programmation fonctionnelle a, depuis peu (NdT : au moment où l'auteur a écrit), regagné l'intérêt du grand public programmeur. Une des raisons à cela est que les *emergent properties *du paradigme fonctionnel sont bien adaptées pour résoudre les défis posés par une industrie glissant de plus en plus vers le multi-cœurs. Mais bien ce que soit une application importante, ce n'est pas la raison qui devrait te pousser à apprendre le paradigme fonctionnel.

La maîtrise du paradigme fonctionnel peut grandement améliorer la qualité du code que tu écris, même dans d'autres contextes. Si tu comprends vraiment et appliques le paradigme fonctionnel, cela se ressentira dans tes designs, qui montreront un plus haut niveau de transparence référentielle ou *referential transparency*.

La transparence référentielle est une propriété très désirable : elle implique qu'une fonction retourne constamment le même résultat pour une même entrée, peu importe quand et où a été invoquée cette fonction. L'idée est que l’évaluation d'une fonction dépende moins, voire idéalement pas du tout, des effets de bords et autres *mutable state*.

Une des causes majeures de problème avec le code impératif est dû aux **variables mutables**. Chaque lecteur a déjà du chercher à comprendre pourquoi l'on n'obtenait pas le bon résultat dans une situation particulière. Une bonne sémantique de visibilité (NdT : la façon dont un langage gère la portée des variables par exemple) aide à limiter ces effets pervers, ou, tout du moins, réduire leur localisation dans le code. Mais le vrai problème serait plutôt les designs qui emploient la mutabilité de façon exagérée.

Ce n'est pas dans notre métier que nous verrons les bonnes pratiques se propager. Beaucoup d'introduction à l'orienté objet promeuvent de tels designs, montrant des exemples de superbes graphiques d'objets à longue durée de vie qui s'appellent leurs mutateurs les uns sur les autres, ce qui est dangereux. Cependant, avec l'aide du **[Test Driven Development](https://fr.wikipedia.org/wiki/Test_driven_development)**, notamment quand on s'efforce de "[Mock Roles, not Objects](http://chrome-extension://oemmndcbldboiebfnladdacbdfmadadm/http://www.jmock.org/oopsla2004.pdf)" (NdT : pardon, mais là je ne sais vraiment pas comment traduire), on peut drastiquement éliminer la mutabilité de notre design.

Le résultat est un design dont les responsabilités sont bien réparties entre les différents composants, dont les fonctions sont plus petites et agissent sur des arguments qu'on leur passe et non sur des variables globales mutables. De cette manière, il y aura moins de problèmes et ils seront plus faciles à corriger, parce qu'il sera plus facile de localiser où une valeur rebelle a bien pu être introduite plutôt que de déduire le contexte particulier dans lequel cette valeur étrange est apparue. Ça ajoute un plus grand degré de transparence référentielle ; aucune méthode ne pourra t'inculquer ces idées aussi bien que le fait d'apprendre un langage fonctionnel.

Bien entendu, cette approche n'est pas la meilleure dans toutes les situations. Par exemple, dans un système orienté objet, cette façon de faire donne de meilleurs résultats avec le **[Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design)*** *(NdT : ou en bon français, développement piloté par le domaine) que pour le développement d'une interface utilisateur.

## Mon mot à moi

Je n'ai pas assez d'expérience avec la programmation fonctionnelle, mais c'est vrai qu'avoir de l'expérience dans d'autres domaines ne peut être que profitable. Donc même si je n'ai pas encore pu pratiquer le DDD, je continue à apprendre Haskell.
