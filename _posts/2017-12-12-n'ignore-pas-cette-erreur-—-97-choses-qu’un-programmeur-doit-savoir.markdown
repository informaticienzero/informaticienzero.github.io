---
layout: default
title:  "N'ignore pas cette erreur — 97 choses qu’un programmeur doit savoir"
date:   2017-12-12 15:05 +0200
categories: blog
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [N'ignore pas cette erreur](http://programmer.97things.oreilly.com/wiki/index.php/Don%27t_Ignore_that_Error%21) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Pete Goodliffe](http://programmer.97things.oreilly.com/wiki/index.php/Pete_Goodliffe).

<!--excerpt-->

* J'étais en train de marcher dans la rue un soir, pour retrouver mes amis dans un bar. Nous n'avions pas pris de bière depuis quelques temps, et j'étais impatient de les revoir. Dans mon empressement, je ne regardais pas où je marchais. J'ai trébuché sur le trottoir et je me suis retrouvé à plat sur mon visage. Je l'ai bien mérité pour ne pas avoir été attentif.

Ça faisait mal à la jambe, mais j'étais pressé de revoir mes amis. Donc je me suis relevé et j'ai continué. Au fur et à mesure que je marchais, ma douleur augmentait. Bien que je l'ai mis d'abord sur le compte du choc, j'ai rapidement compris qu'il y avait quelque chose qui n'allait pas.

Je suis quand même allé au bar. Quand je suis arrivé j'étais à l'agonie. Je n'ai pas eu une bonne nuit, parce que j'étais très distrait. Le matin, je suis allé chez le docteur et j'ai découvert que je m'étais fracturé un os. Si je m'étais arrêté quand j'ai senti la douleur, je me serai épargné beaucoup de dommages supplémentaires, infligés quand je marchais. Sans doute le pire lendemain matin de ma vie.
*

Trop de programmeurs écrivent du code comme ma désastreuse sortie de nuit.

*Erreur, quelle erreur ? Ça n'est pas très grave. Franchement. Je peux l'ignorer*. Ça n'est pas une stratégie gagnante pour écrire du code robuste. En fait, c'est juste de la fainéantise (la mauvaise). Peu importe à quel point tu pense qu'il ne peut y avoir d'erreurs dans le code, tu devrais toujours vérifier, et les *handler*. À chaque fois. Tu n'économises pas de temps si tu ne le fais pas : tu conserves simplement des problèmes potentiels pour le futur.

On remonte des erreurs dans notre code de différentes façons.

* Les **codes de retours** peuvent être utilisés comme résultat d'une fonction pour signaler une erreur. Les codes d'erreurs sont beaucoup trop faciles à ignorer. Rien dans le code ne soulignera le problème. En effet, c'est une pratique standard d'ignorer les retours de certains fonctions C. Combien de fois vérifie-tu la valeur de retour de `printf` ?
* **errno** est une curieuse abbération du C, une variable globale modifiée pour signaler une erreur. C'est facile à ignorer, dure à utiliser, et mène à pleins de problèmes — par exemple, que se passe-t-il quand tu as plusieurs threads appelant la même fonction ? Certaines plateformes nous épargnent cette douleur ; d'autres non.
* Les **exceptions** sont une façon plus structurée, intrégrée au langage, de signaler et prendre en charge des erreurs. Et tu ne peux pas les ignorer. Vraimeht ? J'ai vu beaucoup de code comme ça.
```
try {
    // ...do something...
}
catch (...) {} // ignore errors
```
Si tu ignores une erreur, que tu fermes les yeux et que tu prétende que rien ne va mal, tu coures de grands risques. Exactement comme ma jambe qui a fini dans un état pire que si je m'étais arrêté, ignorer les erreurs peut mener à des *failures* complexes. Règle les problèmes à la première opportunité. 

Ne pas gérer les erreurs mène à :

* Du **code fragile**. Un code remplis de bugs exitants et durs à trouver.
* Du **code non sécurisé**. Les crackers exploitent souvent une mauvaise gestion des erreurs pour pénétrer le système.
* Une **mauvaise structure**. Si, continuellement, il y a des erreurs qui sont fastidieuses à traiter, tu as sans aucun doute une mauvaise interface. Exprime-la de façon à ce que les erreurs soient moins intrusives et leur traitement moins coûteux.

De la même façon que tu devrais checker toutes les erreurs potentielles dans ton code, de même tu dis exposer toutes les conditions menant potentiellement à des erreurs dans tes interfaces. Ne les caches pas en prétendant que tes services seront toujours fonctionnels.

Pourquoi ne controlons-nous pas les erreurs ? Il y a un certain nombre d'excuses communes. Avec lesquelles es-tu d'accord ? Comment les contrer ?

- La gestion des erreurs encombre le flux de code, le rendant plus dur à lire et à extraire le flow normal d'exécution.
- C'est du travail supplémentaite et j'ai la deadline qui s'approche.
- Je sais que cet appel de fonction ne retournera jamais d'erreur (`printf` marche toujours, `malloc` retourne toujours de la mémoire — si ça échoue, c'est que nous avons de gros problèmes…).
- Ce n'est qu'un programme de test, qui n'ira jamais en production.