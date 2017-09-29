---
layout: default
title: "The Pragmatic Programmer - L'éthique d'un développeur pragmatique 1"
date: 2015-08-17 21:40:41
permalink: /:title/
---
Avant d'apprendre à être un développeur pragmatique, il faut examiner ceux qui le sont pour comprendre leur attitude, leur façon de faire et de penser. Le premier chapitre du livre se concentre dessus et le fait bien. En effet, plutôt que de nous jeter à la figure ce que fait un tel développeur ou de nous noyer avec des notions théoriques, ce chapitre regorge de métaphores et d'illustrations. Examinons-les.

## Le chat a mangé le code source

Peut-être que les anglo-saxons font du chat ce que les auteurs de bandes dessinées franco-belges font du chien, ce célèbre mangeur de devoirs ou de bulletins trimestriels (Boule et Bill, si vous m'entendez). En tout cas, dans les deux cas, on se rend bien compte que jeter la culpabilité sur un animal est bien souvent ridicule (et source de gags dans la bande dessinée). Qui irait-voir son supérieur en disans « *Désolé chef, j'étais en train de travailler sur le projet hier mais mon chat a tout mangé* » ? Mais pourtant, de telles excuses sont sorties, d'une façon moins ridicule certes, mais tout aussi désespérante : « *Le code source a disparu suite au crash du disque dur sur lequel il était stocké.* »

La première idée de ce chapitre est **qu'un bon programmeur pragmatique ne fournit pas de mauvaises excuses mais plutôt qu'il doit fournir des solutions**. S'il a accepté une responsabilité, il l'assume ; ainsi, même s'ils peuvent porter une partie de la faute, le développeur pragmatique ne va pas jetter la pierre sur un collègue, un chef, un logiciel, une machine, etc. Il va même plus loin en se demandant, avant même d'aller voir les personnes concernées, ce qu'il peut faire pour faire avancer la situation.

*   « *Le code source a disparu suite au crash du disque dur sur lequel il était stocké.* »
*   « *Le disque dur a planté hier soir, mais une équipe est déjà dessus et nous avons pu récupérer 90% du code source.* »

Quelle version préférez-vous ? La réponse est évidente. Pour arriver à la deuxième phrase, un développeur pragmatique doit toujours se poser des questions.

*   Ce que je vais dire semble-t-il raisonnable ou bien stupide ? Est-ce que mes propos démontrent mon sérieux et mes efforts ou bien mon manque de prévision et de clairvoyance ?
*   Comment la personne en face de moi va-t-elle le prendre ?
*   Que vont vraisemblablement dire ces personnes ? Vont-elles proposer des solutions auxquelles je n'ai pas encore pensé ?

Mais l'histoire de notre chat glouton comporte un deuxième enseignement, plus personnel. En effet, un programmeur pragmatique sait que **personne, excepté lui, ne prendra en main sa carrière, ses projets et son travail quotidien**. S'il veut rester efficace, il sait qu'il a un travail de veille technologique à faire. Il n'aura ainsi pas peur de demander des formations ou des stages plutôt que d'attendre qu'on vienne lui proposer. Dans ses projets et son travail, il sait que c'est à lui de produire des efforts pour obtenir le meilleur possible. Il est tout à fait conscient qu'il a sa part à faire et que personne ne la fera pour lui.

Cela va lui demander une certaine **humilité** et de la **modestie**. L'humilité va lui permettre d'accepter et de surmonter la peur de l'ignorance et de l'erreur ; aussi pragmatique soit-il, un programmeur est un humain, pétri d'erreurs et à la connaissance limitée et il y aura toujours un problème auxquel il n'aura pas la réponse. La modestie, c'est à dire la conscience de ses limites, va l'aider à n'avoir ni honte ni gêne de demander de l'aide. C'est aussi un moyen d'apprendre, d'approfondir, de découvrir ce que nous ne savions pas avant. Seuls les gens orgueilleux et imbus d'eux-mêmes ne veulent pas se remettre en question et se reposent sur leurs acquis.

## L'entropie logicielle

L'entropie est, selon le Larousse, ce qui caractérise l'état de désordre d'un système et plus précisément dans notre cas, d'un développement. Plus cette entropie est élevée, plus le développement est tortueux, les évolutions difficiles, les corrections pénibles. Le problème, c'est que le désorde peut commencer avec un minuscule détail qui va vite faire effet boule de neige. C'est **le syndrome de la vitre brisée**.

Ce syndrome s'observe dans les grandes villes. Quand un bâtiment est inoccupé, s'il est toujours entretenu, si les moindres soucis sont réparés, tout va bien et le bâtiment perdure. Cependant, si une seule vitre vient à être brisée sans être réparée, alors un sentiment d'abandon va transparaître et les dégradations vont s'enchaîner : une deuxième vitre, une troisième, des tags, l'intérieur sacagé, les portes défoncées, etc jusqu'à un tel point où détruire le bâtiment sera préférable à le réparer. *HS : ce phénomène s'observe aussi avec l'urbex ; dès qu'un lieu préservé est rendu public, il est très souvent rapidement dégradé.*

Quel développeur n'a jamais eu à faire face à ce genre de projet qui prend l'eau, ou tout du moins n'en a pas entendu parler ? Mais comment cela se traduit-il concrètement ? Cela peut commencer par des choses très simples : du code mort, redondant, sale, une dette technique qui s'alourdit, etc. Même les meilleurs développeurs, sous le coup de la pression, de la fatigue, de délais serrés, de la flemmardise, peuvent en quelque sorte « *briser une vitre* » et faire couler un projet si bien parti. Pour s'en prémunir, il convient de remplacer le moindre problème que l'on trouve dans le code et ce, sans tarder. On peut penser aux revues de code et aux pair-programming ou tout autre moyen qui n'aurait pas encore été inventé, bref, tout est bon pour corriger le code.

## Soupe de cailloux et torture de grenouille

*Trois soldats affamés sont sur le chemin du retour. Quand ils aperçurent le village qui se dressait à l'horizon, leur esprit reprit vie : ils étaient sûrs qu'ils pourraient manger un petit quelque chose. Mais à leur arrivée, toutes les portes étaient verrouillées et tous les volets fermés. Les villageois, après des années de guerre, tenaient à protéger leur nourriture.*

*Pas découragés pour autant, les soldats mirent une marmite sur le feu qu'ils remplirent de trois gros cailloux. Les villageois, intrigués, commencèrent à sortir à leur rencontre.*

*"C'est une soupe de cailloux" expliquèrent les soldats. "Mais c'est tout ? Rien d'autre dedans ?" demandèrent les villageois. "Oui, absolument. Enfin, avec quelques carottes, ça a plus de goût." Ni une ni deux, un villageois courrait chez lui et revint avec un panier de belles carottes.*

*"Est-ce vraiment tout ? Eh bien", répondirent les soldats, "un peu de pomme de terre donnerait du corps à la soupe". Un autre villageois fit l'aller-retour jusqu'à chez lui.*

*Les soldats listèrent de cette manière une liste d'ingrédients qui relèveraient le goût de la soupe : de la viande, des poireaux, du sel, des herbes et des épices. Pour chaque ingrédient, un villageois différent amenait sa contribution.*

*En fin de compte, une soupe bien garnie et nourrissante fut produite par l'ensemble des personnes. Les soldats n’eurent qu'à retirer les cailloux pour que tout le monde puisse profiter d'un repas riche tel que personne n'en avait eu depuis longtemps.*

Les soldats sont ici des catalyseurs qui amènent** le groupe à produire quelque chose que les individus seuls n'auraient pu faire**. Il en résulte que tout le monde gagne. Un programmeur pragmatique va s'efforcer d'être ce catalyseur.

Le livre donne cette application : on veut faire quelque chose, une modification et on sait qu'elle est nécessaire. Il y a cependant des risques de perdre du temps dans des réunions et des validations si on demande de but en blanc à modifier le tout. L'astuce va alors consister à bien faire une petite modification, à la montrer aux autres et à dire "C'est sûr, ce serait mieux si ... mais bon, ce n'est pas trop important". Le livre dit que les gens vont rapidement commencer à demander comment implémenter ce fameux truc pas important puis vont le faire.

Je ne sais pas si dans la réalité cela marche à 100% et d'ailleurs je ne suis pas convaincu (peut-être parce que je n'ai jamais pratiqué). Par contre, je suis sûr d'une chose, c'est qu'il est plus facile à quelqu'un de s'associer à une opération ayant déjà un certain succès (une petite modification réussie) que de se lancer dans l'inconnu (une grosse modification dont on a aucune certitude sur le succès).

Cette métaphore contient un deuxième enseignement :** ne pas trop se laisser absorber par des détails** comme les villageois l'ont été par les cailloux. On commence par une nouvelle fonctionnalité, puis par une autre, et encore une autre jusqu'à s'être peu à peu éloigné de la demande de départ, parce que l'on a été trop attentif aux détails et qu'on a perdu de vue l'ensemble.

Connaissez-vous la grenouille qui passe à la bouilloire ? Si on jette une grenouille dans de l'eau chaude, elle en sort immédiatement pour ne pas mourir. Si par contre on la met dans de l'eau froide dont la température va lentement augmenter, la grenouille ne se rend compte de rien et meurt bouillie. L'idée est la même que celle du paragraphe précédent : **garder un œil sur le projet en général** et pas que sur le travail accompli par nous-mêmes.

Enfin, il convient de bien réfléchir au **type de catalyseur que l'on veut être** : allons-nous faire du bien tels des cailloux dans la soupe, ou bien allons-nous avoir des conséquences fâcheuses telle la grenouille tuée par l'eau ? Ceci, cher lecteur, est laissé à ton propre jugement.

Voilà pour la première partie de l'article. Les trois points suivants du chapitre 1 seront évoqués dans un autre article. En attendant, bonne mise en pratique !
