---
layout: default
title: "Pas de tendresse avec les données de test — 97 choses qu’un programmeur doit savoir"
date: 2017-09-04 15:31:26
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Pas de tendresse avec les données de test](http://programmer.97things.oreilly.com/wiki/index.php/Don%27t_Be_Cute_with_Your_Test_Data) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par[ Rod Begbie](http://programmer.97things.oreilly.com/wiki/index.php/Rod_Begbie "Rod Begbie").

<!--excerpt-->

> Il se faisait tard. J'ai placé dans un répertoire approprié des données pour tester le layout de la page sur laquelle je travaillais.
> 
> J'ai utilisé les membres de *The Clash* comme noms d'utilisateurs. Les noms de compagnies ? Des titres de chansons des *Sex Pistols* feraient l'affaire. Maintenant, il me fallait des symboles boursiers *—* des simples mots de quatre lettres en majuscule.
> 
> J'ai utilisé **ces** mots de quatre lettres.
> 
> C'était inoffensif. Simplement quelque chose pour m'amuser, et peut-être amuser les autres développeurs le lendemain, avant de câbler la vraie source de données.
> 
>  
> 
> Le matin suivant, un chef de projet prenait des captures d'écran pour une présentation.

L'histoire de la programmation est remplie de petites histoires de guerre comme celle-là. Des choses que les développeurs et les designers ont fait, « qui jamais ne se verraient », ont sans crié gare été rendues publiques.

Le type de fuite peut varier mais, quand ça arrive, ça peut être mortel pour la personne, l'équipe ou l'entreprise responsable. Voici des exemples.

- Durant une réunion de présentation, un client clique sur un bouton qui n'a pas encore été implémenté. Il se voit répondre « Ne reclique jamais, espèce de crétin. »
- Un programmeur, chargé de maintenir un système legacy, s'est vu dire d'ajouter un message d'erreur, et décide pour l'alimenter d'utiliser un système de logging de derrière le rideau. Les utilisateurs vont maintenant découvrir des messages du type « Holy database commit failure, Batman! » quand une erreur arrive.
- Quelqu'un mélange les interfaces de test et de production et s'amuse en ajoutant des données. Les clients vont donc remarquer la présence, à 1 million de dollars, le « masseur personnel en forme de Bill Gates » sur ton site de vente en ligne.

Pour transposer le vieil adage « un mensonge peut traverser la moitié de la planète pendant que la vérité met ses chaussures » dans notre monde moderne, une maladresse peut être diffusée sur Dugg, Twitter et Flibflarb avant même qu'une personne à même de réparer ça ne soit réveillé.

Même ton code source n'est pas à l'abris. En 2004, quand une archive contenant le code de Windows 2000 a fait son chemin sur les réseaux de partages de fichiers, certaines personnes ont remarqué les jurons, insultes et [autres joyeusetés](http://atdt.freeshell.org/k5/story_2004_2_15_71552_7795.html). (Ndt : le commentaire `// TERRIBLE HORRIBLE NO GOOD VERY BAD HACK` a depuis été adopté par le rédacteur original de l'article).

En résumé, quand tu écris du texte dans ton code — que ce soit des commentaires, des logs, des dialogs ou des données de test — demande-toi toujours l'impression que ça dégagera si cela devenait public. Ça t'épargnera bien des visages en colère.

## Mon mot à moi

Un peu d'humour ne fait jamais de mal. Je suis le premier à avoir ajouté quelques petis easter eggs dans le code, comme un chat ASCII-art dans les commentaires du fichier d'installation de l'environnement de développement. Mais il faut par contre rester respectueux et ne pas tomber dans la blague lourde ou gamine. On peut s'amuser un peu avec les données de test, mais effectivement, la réflexion à mener pour savoir si l'on serait fier de les voir rendues publiques est importante à mener.

Il faut aussi que les données soient de qualités, pas seulement des valeurs bidons entrées au hasard, mais ça, c'est une autre question.