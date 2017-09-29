---
layout: default
title: "Un commentaire à propos des commentaires — 97 choses qu’un programmeur doit savoir"
date: 2016-11-29 12:39:59
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Un commentaire sur les commentaires](http://programmer.97things.oreilly.com/wiki/index.php/A_Comment_on_Comments) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Cal Evans](http://programmer.97things.oreilly.com/wiki/index.php/Cal_Evans).

<!--excerpt-->

Lors de mon premier cours de programmation à l'université, mon professeur distribua deux feuilles de code en BASIC. Sur le tableau, la consigne était « Écrire un programme récupérant et faisant la moyenne de 10 scores de bowling. » Puis le professeur quitta la salle. À quel point était-ce dur ? Je ne me rappelle pas de ma solution finale, mais je suis sûr qu'elle contenait une boucle `FOR/NEXT` et qu'elle ne faisait pas plus de 15 lignes de long. Chaque feuille — pour les enfants qui lisent, oui, on avait l'habitude d'écrire du code à la main bien avant de pouvoir le rentrer sur un ordinateur (NdT : pour moi aussi, j'ai eu des contrôles de code sur papier, mais dans les années 2010...) — permettait d'écrire environ 70 lignes. J'étais **très confus** en me demandant pourquoi le prof nous donnait deux feuilles. Vu que mon écriture avait toujours été atroce, j'ai utilisé la deuxième pour recopier soigneusement mon code, espérant gagner quelques points supplémentaires pour la présentation.

À ma grande surprise, quand j'ai reçu mon contrôle corrigé au début du cours suivant, j'avais à peine la moyenne (ce fut un présage pour moi pour le reste de mon temps à l'université). Griffonnée dans le haut de ma copie si propre, « **Pas de commentaire ?** »

Ça ne suffisait pas que le prof et moi sachions ce que le programme était supposé faire. Une partie de l'objectif du contrôle était de me faire apprendre que **mon code doit s'auto-expliquer au programmeur qui viendra après moi**. C'est une leçon que je n'ai jamais oublié.

Les commentaires ne sont pas mauvais. Ils aussi indispensable à la programmation que des structures conditionnelles ou des boucles. La plupart des langages modernes ont un outil similaire à javadoc qui va parser des commentaires correctement formatés afin de produire automatiquement une documentation d'API. C'est un très bon début, mais ce n'est pas assez.  À l'intérieur même du code, il devrait y avoir **des explications sur ce que le code est sensé faire**. Coder en suivant le vieil adage « Si c'est dur à écrire, c'est dur à lire » ne rend service ni à ton client, ni à ton employeur, ni à tes collègues ni à ton futur toi.

D'un autre côté, tu peux aller trop loin en commentant. Vérifie que **tes commentaires clarifient le code au lieu de l’obscurcir**. Saupoudre ton code avec des commentaires pertinents expliquant ce que le code est sensé accomplir. Les commentaires en en-tête devraient donner, à n'importe quel programmeur, suffisamment d'informations pour utiliser le code sans avoir à le lire, tandis que les commentaires dans le code vont aider le prochain développeur à le corriger ou l'étendre.

Une fois, dans un ancien travail, je n'étais pas d'accord avec un décision de design faîte par ceux avant moi. Me sentent d'humeur sarcastique, comme le sont la plupart des jeunes programmeurs, j'ai collé l'email m'instruisant d'utiliser leur design dans le bloc de commentaire d'en-tête. Il se trouve que les managers de cette boutique ont fait une revue de code quand il a été commité. Ce fut ma première introduction au terme de *career-limiting move* (NdT : terme anglo-saxon désignant quelque chose qui peut valoir le renvoi ou abréger rapidement une carrière).

## Mon mot à moi

Pour approfondir, de mon point de vue, les commentaires ne doivent pas paraphraser le code mais plutôt** en expliquer la logique derrière**, expliquer le pourquoi du code et non le comment. On peut aussi s'en servir pour **commenter une portion complexe** mais attention à faire la différence entre une portion fondamentalement complexe et une code complexe parce que mal écrit ou implémenté.

Les commentaires servent aussi, on ne va pas se le cacher, à **isoler du code**, lors d'un débug par exemple. Mais ça doit rester une solution temporaire, sinon c'est vraiment moche (parole de codeur).

*   [L'article Wikipédia (en) sur les commentaires](https://en.wikipedia.org/wiki/Comment_(computer_programming)).
*   [Un article de blog à propos des commentaires](http://www.hongkiat.com/blog/source-code-comment-styling-tips/).

[![Un développeur se fait reprendre pour avoir commité du code commenté alors qu'il utilise SVN.](http://www.commitstrip.com/wp-content/uploads/2013/08/Strips-Commentaire-de-sauvegarde-600-final.jpg) Isoler du code avec des commentaires mais le commiter quand même.
