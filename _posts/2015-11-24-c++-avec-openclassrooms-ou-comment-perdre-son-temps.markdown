---
layout: default
title: "C++ avec OpenClassrooms, ou comment perdre son temps"
date: 2015-11-24 19:36:57
permalink: /:title/
---
J’aime le C++. C’est un de mes langages préférés. Malheureusement, c’est un **langage trop souvent très mal enseigné**. Je pense à tous ces cours datant de Mathusalem faits par des personnes qui croient encore que le C++ n’est qu’un bête sur-ensemble de C avec des classes (un peu comme [cette abomination là](https://openclassrooms.com/courses/du-c-au-c), qui vient ... du même site, tiens donc). Mais il y a aussi d’autres cours qui, sans tomber dans cet extrême, enseignent de mauvaises pratiques, méprisant l’évolution du langage depuis 2011 et forçant ainsi un débutant à désapprendre pour reconstruire avec des ressources de qualité et à jour. Ça, c’est le cas du tutoriel d’OpenClassrooms.

Avant toute chose : **on s’adresse à des débutants qui veulent apprendre à programmer**, pas à des professionnels qui connaissent bien la question et savent quand enfreindre certaines règles. Donc pas la peine de me sortir que les pointeurs nus sont utiles pour interfacer avec le C, je le sais et ce n’est pas l’objet de ce post.

# Le naturisme des pointeurs et la dynamique allocation

Les pointeurs sont utiles en C++, ne nous mentons pas. Mais les pointeurs nus en C++, dans 99.5% des cas, c’est sale. Ce que j’appelle un pointeur nu ? C’est l’équivalent de *raw pointer* en anglais ; ce sont les pointeurs tels qu’on les voit en C. Mais qu’est-ce qui les rend sales ? L’existence de moyens de s’en passer presque tout le temps.

Déjà, il y a **les références**, qui nous simplifient la vie quand on fait un passage par argument, par exemple. Ensuite, il y a **les pointeurs intelligents** qui s’occupent pour nous de libérer la ressource acquise (parce qu'ils utilisent le merveilleux concept du [RAII](2015-08-02-simplifier-la-gestion-de-la-mémoire-en-c++-avec-raii.markdown). Enfin, **la bibliothèque standard**, avec tout ce qu’elle fournit, comme `std::vector` par exemple, nous permet de bannir les allocations dynamiques manuelles ; dans un langage à exceptions comme le C++, la gestion manuelle de la mémoire est un cauchemar aux vues du nombre possible de chemins d’exécutions différents.

Les allocations dynamiques n’ont d’ailleurs **pas leur place aussi tôt dans un cours**. Le débutant peine encore à jouer avec les fonctions et on devrait lui asséner un chapitre dessus ? Dans un cours de C, c’est déjà discutable, malgré l’importance de l’allocation dynamique. Mais dans un cours de C++, aux vues des arguments cités précédemment, **c’est une aberration**, ni plus ni moins ! Et non content d’utiliser `new` / `delete` sans vergogne, ce cours présente des code bogués. [Un exemple](https://openclassrooms.com/forum/sujet/erreur-heritage-poo#message-88917832).

# Des tableaux anciens

D’ailleurs, si liés aux pointeurs qu’ils sont en C, les tableaux entre crochets devraient être quasi-systématiquement **bannis d’un code C++ propre et moderne**. La bibliothèque standard nous fournit de nombreux conteneurs fiables, sécurisés et pratiques. Et qu’on ne vienne pas me dire que l’on ne peut pas avoir de tableau statique depuis que la norme C++11 a introduit `std::array` ! Donc les tableaux C, qu’ils soient statiques ou dynamiques, on a mieux en C++ donc on oublie.

# Les notions du fond de la caverne

Ou plus exactement, les trucs importants à savoir qui ne sont abordés qu’à la fin du tutoriel. Exemple ? **La gestion des erreurs**. C++ est un langage à exceptions ; celles-ci peuvent nous claquer dans les jambes mais on ne les voit que dans **un obscur chapitre d’une partie appelée « Notions avancées »**. Pourtant, rien n’est plus basique et la gestion des erreurs devrait faire partie des bases. Et ce n’est pas la seule tare de ce chapitre.

En effet, **il présente les exceptions là où n’elles n’ont pas de raison d’être** : les erreurs de programmation. Ainsi, si je passe un nombre négatif à une fonction calculant une racine carrée et qui attend un nombre positif, c’est une erreur de programmation et je suis le seul fautif. Une assertion est beaucoup plus adaptée : c’est la programmation par contrats. Les exceptions sont à utiliser dans le cas de ressources à acquérir pouvant potentiellement échouer, comme se connecter à un serveur ou tenter d’ouvrir un fichier par exemple. Enfin, inutile d’en rajouter une couche sur **toute la SL qui est passée sous silence** et ne sort qu’à la partie IV du cours, alors qu’elle est tout simplement essentielle à un code de qualité.

Et quant à Qt, **pas un mot sur le système de gestion de mémoire parent/enfants**, qui est juste crucial pour faire un application Qt qui tient la route.

# Un cours du siècle passé

Il n’a **jamais été mis à jour pour C++11 et C++14, **alors que** C++17 est sur le point de sortir**. Lambda ? `constexpr` ? La *move-semantic* ? L’inférence de type ? Initialisation avec `{}` ? Inconnus au bataillon. C’est dommage, parce que certains de ces ajouts sont vraiment utiles. Et le mettre à jour pourrait ajouter une sacrée plus-value, vu le peu de cours de qualité et à jour sur l’internet francophone.

Autre exemple : le chapitre sur les fichiers, **totalement périmé**. Et ne parlons pas de Qt, **dont la partie n'a pas été mise à jour pour la version 5**, qui apporte notamment des changements profonds niveau connexion des slots et des signaux.

# La conception bafouée et foulée aux pieds

Là, c’est le pire du pire, l’erreur qui ne pardonne pas et qui condamne ce cours comme étant **nul et à fuir**. Même les autres reproches ne sont rien face à ça. Prenons par exemple le cas du RPG, fil rouge de la partie II sur l’OO. Nous sommes bien d’accord qu’un personnage est quelque chose d’unique ; même si deux guerriers ont le même nom et un même type d’arme, ce sont néanmoins deux personnes distinctes. On parle de **sémantique d’entité**. Une classe à sémantique d’entité ne doit donc pas être copiée ni affectée, ce qui implique de supprimer le constructeur par recopie et `operator=()`. Mais dans le cours d’OC, pas de soucis ! On peut le faire sans problème. C’est même utilisé dans le cours ([ici](https://openclassrooms.com/forum/sujet/erreur-heritage-poo#message-88917674) pour plus détails). Ce [long topic](https://openclassrooms.com/forum/sujet/comment-structurer-un-jeu-en-c) détaille d'ailleurs les inconvénients de cette méthode et comme mieux faire.

De plus, rien, **absolument rien sur comment bien concevoir un programme**. Je ne parle même pas des designs patterns (qui n’ont pas leur place ici) mais des règles de base, celles qui sont juste essentielles : SOLID et la loi de Demeter. Au contraire, [le livre de Philippe Dunski](https://www.d-booker.fr/programmation-et-langage/157-coder-efficacement.html) les aborde, même s’il est vrai que ce n’est pas un livre d’apprentissage du C++. Et franchement, **quel intérêt à enseigner à des débutants à programmer s’ils doivent tout désapprendre ensuite et suivre un autre cours pour enfin coder correctement** ? Voilà pourquoi je m’oppose au cours d’OC.

# Et pour quelques liens de plus

Parce que je ne suis pas le seul à le dire, et que ce n'est pas la première fois que le sujet est abordé sur les forums d'OC.

1.  [Début du cours C++](https://openclassrooms.com/forum/sujet/debut-du-cours-c#message-89384736)
2.  [Programmer avec le langage C++](https://openclassrooms.com/forum/sujet/programmez-avec-le-langage-c-7)
3.  [Cours de C++ / Qt plus à jour](https://openclassrooms.com/forum/sujet/cours-de-c-qt-plus-a-jour#message-89238532)
4.  [Les notions de C++](https://openclassrooms.com/forum/sujet/les-notions-de-c#message-89197790)
5.  [Problème programmation C++](https://openclassrooms.com/forum/sujet/probleme-programation-c-1?page=2#message-89127817)
6.  [Mauvais cours de C++](https://openclassrooms.com/forum/sujet/mauvais-cours-de-c)
7.  [Le tutoriel de m@téo21 est-il mauvais ?](https://openclassrooms.com/forum/sujet/le-tutoriel-de-m-teo21-est-il-mauvais?page=1)
8.  [MOOC C++](https://openclassrooms.com/forum/sujet/mooc-c?page=7#message-86545267)
9.  [Les cours d'Openclassrooms](https://openclassrooms.com/forum/sujet/les-cours-d-openclassrooms#message-89418649)
10.  [Passer du C au C++](https://openclassrooms.com/forum/sujet/passer-du-c-au-c-1)
11.  [Problème avec le cours C++](https://openclassrooms.com/forum/sujet/probleme-avec-le-cours-c#message-91042239)
12.  [Cours C++ : Les pointeurs - Question](https://openclassrooms.com/forum/sujet/cours-c-les-pointeurs-question)
13.  [Cours C++ : différences entre les versions de C++](https://openclassrooms.com/forum/sujet/cours-c-40)
14.  [Quand utiliser les pointeurs](https://openclassrooms.com/forum/sujet/quand-utiliser-des-pointeurs-4), par un membre imbu de lui même qui ne supporte pas qu'on lui donne des conseils mais qui m'offre pleins de posts montrant la piètre qualité de ce cours.
15.  Encore une fois, notre [membre imbu de lui-même](https://openclassrooms.com/forum/sujet/quel-livre-choisir-pour-apprendre-le-c) continue de promouvoir le cours d'OC malgré, une fois de plus, les arguments développés des experts C++ du forum.
16.  [Pas de pointeur](https://openclassrooms.com/forum/sujet/declarer-un-nombre-defini-de-chaines-de-caracteres#message-85150640), un post où Ksass`Peuk nous explique comment réduire à un quasi-néant l'utilisation de pointeurs nus grâce à la bibliothèque standard C++.