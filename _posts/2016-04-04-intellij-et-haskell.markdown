---
layout: default
title: "IntelliJ et Haskell"
date: 2016-04-04 14:49:40
permalink: /:title/
---
J'aime vraiment cet IDE, parce qu'il offre un plugin pour utiliser Haskell, en plus de celui [pour D](http://www.informaticienzero.com/intellij-et-le-langage-d/). J'espère que le fait d'avoir un IDE va me remotiver à me plonger dans ce langage et dans le paradigme fonctionnel en général. Pour ceux qui sont intéressés, voilà comment j'ai fait.

<!--excerpt-->

*   **Installer IntelliJ et Haskell**. Bon c'est la base ça, mais précisons-la quand même.
*   **Mettre à jour cabal**, le gestionnaire de dépendances fourni avec Haskell. Cela se fait avec la commande `cabal update`
*   Une fois que c'est fait, **installer ghc-mod** avec la commande `cabal install ghc-mod`
*   Dans IntelliJ, installer le **plugin Haskell**, qui ce trouve [ici](https://plugins.jetbrains.com/plugin/7453?pr=idea).
*   Quand ceci est fait, lancer IntelliJ et aller dans *File -> Project Structure -> SDK*. Là, **créer un nouveau SDK** avec le petit plus vert. Normalement, GHC est proposé. Dans mon cas, le chemin du SDK est `/usr/local/haskell/ghc-7.10.3-x86_64`

Il ne reste plus qu'à créer un nouveau projet, le configurer pour utiliser le SDK créé et profiter. Maintenant, plus d'excuse pour ne plus faire de Haskell !
