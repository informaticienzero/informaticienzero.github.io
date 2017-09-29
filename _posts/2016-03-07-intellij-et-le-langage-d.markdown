---
layout: default
title: "IntelliJ et le langage D"
date: 2016-03-07 21:45:01
permalink: /:title/
---
Découvrir [le langage D](http://dlang.org/) fait partie de mes objectifs de l'année. Mais ce langage manque d'un bon IDE. Quoique, avec IntelliJ, extensible avec des plugins, [ce n'est plus un problème](https://github.com/kingsleyh/DLanguage). Enfin presque.

<!--excerpt-->

L'installation c'était bien passé, j'avais l'auto-complétion, la coloration syntaxique, etc, mais la compilation crashait avec une vilaine erreur du genre "**Module Main is in file 'main.d' which cannot be read**", ce qui est embêtant quand on sait que tout est bien configuré et installé.

En fait, l'erreur venait du fait que je créais un projet D sans dub, qui est un gestionnaire de dépendances et qui apparemment sait indiquer au compilateur dans quel dossiers sont nos fichiers à compiler. Tout est bien dans le meilleurs des mondes puisque maintenant, ça marche impec !