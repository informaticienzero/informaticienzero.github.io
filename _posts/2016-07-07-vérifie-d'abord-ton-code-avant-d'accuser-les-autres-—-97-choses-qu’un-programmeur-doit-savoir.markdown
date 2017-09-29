---
layout: default
title: "Vérifie d'abord ton code avant d'accuser les autres — 97 choses qu’un programmeur doit savoir"
date: 2016-07-07 18:33:29
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Vérifie d'abord ton code avant d'accuser les autres](http://programmer.97things.oreilly.com/wiki/index.php/Check_Your_Code_First_before_Looking_to_Blame_Others) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Allan Kelly](http://programmer.97things.oreilly.com/wiki/index.php/Allan_Kelly).

<!--excerpt-->

Les développeurs — nous tous ! — avons bien souvent du mal à croire que ce soit notre code qui pète. C'est tellement improbable qu'en fait, pour une fois, c'est sans doute le compilateur qui est en faute.

C'est vrai que dans de (très) rares cas, le code peut être cassé par un bug dans le compilateur, l'interpréteur, l'OS, le serveur, la base de données, le gestionnaire de mémoire ou n'importe quelle autre pièce logicielle. Oui, ces bugs existent, mais ils sont **bien moins courants que l'on aime y croire**.

Une fois, j'ai eu un véritable problème avec un compilateur qui créeait un bug en optimisant une variable de boucle, mais j'ai imaginé que mon compilateur ou mon OS avaient des bugs bien plus souvent. J'ai perdu beaucoup de temps, du temps de support, mais également de management tout ça pour me sentir toujours un peu plus bête à mesure que je m’apercevais que les erreurs venaient de mon côté.

En se basant sur le fait que des outils sont très usités, matures et employés dans différentes stacks technologiques, il n'y a que **peu de raison de douter de leur qualité**. Bien sûr, si l'outil est en *early release*, ou s'il n'est utilisé que par peu de personnes, ou si ce n'est qu'un obscur programme Open Source rarement téléchargé, en version 0.1, on peut avoir de bonnes raisons de suspecter le logiciel (et pour être équitable, une version alpha d'un logiciel commercial est tout autant suspecte).

Étant donnée la rareté des bugs dans les compilateurs, tu ferais bien mieux de **consacrer ton temps et ton énergie à chercher l'erreur dans ton code** plutôt que d'accuser le compilateur. Tous les conseils usuels de débogage s'appliquent, dont isole le problème, bouchonne les appels (NdT : *stub out the calls*), entoures-les de tests ; vérifie les conventions d'appel (NdT : *calling conventions*), les bibliothèques partagés et les numéros de version ; explique-le à d'autres personnes ; regarde si la pile se corrompt ou si un type n'est pas adéquat ; essaye le code sur différentes machines et différentes configurations de build, comme debug et release.

Remets en question tes hypothèses ainsi que celles des autres. Les outils de différents fournisseurs peuvent avoir d'autres hypothèses en leur sein — de même pour plusieurs outils du même fournisseur. Quand quelqu'un remonte un problème que tu ne peux pas reproduire, va les voir et regarde ce qu'ils font. Peut-être accomplissent-ils quelque chose auquel tu n'as pas pensé, ou dans un ordre différent.

Comme règle personnelle, s'il y a un bug que je n'arrive pas à cerner et que je commence à penser que c'est le compilateur, alors il est temps de regarder si la pile est corrompue. C'est particulièrement vrai si ajouter du code de traçage déplace le problème.

Les problèmes multi-threadés sont une autre source de bugs engendrant cheveux gris et cris contre l'ordinateur. Toutes les recommandations visant à garder un code simple prennent encore plus de poids dans un système multi-threadé. Le débugging et les tests unitaires ne sont pas aussi efficaces pour dénicher les bugs, donc un design simple reste la meilleure des protections.

En conclusion, avant de te dépêcher d'accuser le compilateur, rappelle-toi le conseil de Sherlock Holmes : « **Une fois qu’on a éliminé l’impossible, ce qui reste, aussi improbable que cela soit, doit être la vérité** », à préférer à celui de [Dirk Gently](https://fr.wikipedia.org/wiki/Dirk_Gently) : « Une fois qu'on a éliminé l'improbable, ce qui reste, aussi impossible que cela soit, doit être la vérité. »

## Mon mot à moi

De mon côté, c'est plus le système d'exploitation que le compilateur que j'avais tendance à accuser à mes débuts. Maintenant, avec le recul, je partage totalement le point de vue de l'auteur. Comment un compilateur ou un système d'exploitation éprouvé, mature, fruit du travail de centaines de développeurs compétents, pourrait-il être à l'origine des bugs dans 99,9% des cas ? J'essaye au contraire d'être humble et d'admettre que c'est sans aucun doute mon code le responsable.

La seule exception ? Visual Studio 2008. Il est super lent, fait parfois crasher le PC et là je suis certains que c'est pas mon code le responsable. :)
