---
layout: default
title: "C++ avec OpenClassrooms, ou comment perdre son temps"
date: 2015-11-24 19:36:57
permalink: /:title/
---
J’aime le C++. C’est un de mes langages préférés. Malheureusement, c’est un **langage trop souvent très mal enseigné**. Je pense à tous ces cours datant de Mathusalem faits par des personnes qui croient encore que le C++ n’est qu’un bête sur-ensemble de C avec des classes (un peu comme [cette abomination là](https://openclassrooms.com/courses/du-c-au-c), qui vient... du même site, tiens donc).

En plus de bien souvent mépriser l'évolution de C++ depuis 2011, ils enseignent de mauvaises pratiques de programmation orientée objet. Tout cela fait qu'un débutant qui a suivi un tel cours doit ensuite **lutter pour désapprendre** ce qu'il croyait être bien. C’est le cas du tutoriel d’OpenClassrooms.

<!--excerpt-->

Avant toute chose : **on s’adresse à des débutants qui veulent apprendre à programmer**, pas à des professionnels qui connaissent bien la question et savent quand enfreindre certaines règles. Donc pas la peine de me sortir que les pointeurs nus sont utiles pour interfacer avec le C, je le sais et ce n’est pas l’objet de ce post.

# La première partie peut-elle être sauvée ?

Je laisse répondre @koala01, qui a écrit un excellent post [ici](https://openclassrooms.com/forum/sujet/cours-de-c-ocr-de-mathieu-n-et-matthieu-s#message-93693469) mais que je me permet de recopier, pour la postérité.

> > Tous n'est pas à jeter , certaines choses sont vraies mais malheureusement beaucoup de choses sont fausses
> 
> Il n'y a qu'une seule chose qui soit réellement correcte, c'est la syntaxe.
> 
> Encore une chance, sinon tes programmes ne compileraient tout simplement pas. ;)
> 
> Mais, pour le reste…
> 
> - Les images aidant au choix de Code::Blocks n'ont jamais que 5 versions de retard. :p
> - Dans les images, justement, lors de la création du projet, il conseille de décocher la case "create Debug configuration", alors que c'est justement sur cette configuration que tu vas passer le plus de temps (la configuration "release" ne sera utilisée qu'une fois que tout fonctionne)
> - Il te parle de Visual Studio 2010.  Il n'y a jamais eu que cinq versions depuis, mais c'est pas grave, n'est-ce pas ?
> 
> Et ce n'était que mes constatation pour la première page.  Passons à la deuxième.
> 
> J'ai bien conscience que le code qu'il nous montre est le code automatiquement généré par Code::Blocks,  Mais regardons le d'un peu plus près!  Il prend la forme de
> 
> ```c++
> #include <iostream>
>  
> using namespace std;
>  
> int main()
> {
>     cout << "Hello world!" << endl;
>     return 0;
> }
> ```
> 
> Heu…
> 
> Savais tu que la directive `using namespace std;` n'a été fournie par le comité que parce qu'il a décidé, en 1998, de faire passer l'ensemble de la bibliothèque standard dans l'espace de nom `std`, mais que, du coup, toute la base de code qui avait déjà été écrite risquait de ne plus compiler, pour la simple et bonne raison que la bibliothèque standard se trouvait avant dans l'espace de noms global ?
> 
> Du coup, cette directive a été créée pour permettre aux projets "anciens" de continuer à compiler "avec un minimum de modifications": on plaçait cette directive dans un fichier d'en-tête inclus partout ailleurs (par exemple, dans le fichier `config.h`, qui est souvent présent), et "roulez jeunesse".
> 
> Oui, mais, on peut difficilement prétendre que le code que l'on écrit aujourd'hui (en 2020!!!) date ... d'avant 1998, nous sommes d'accords?  Et puis, cette directive pose bien plus de problèmes qu'elle n'en résout :p (je t'en parlerai si tu y tiens).
> 
> Ensuite, il parle de la directive `#include`. C'est sympa… J'aurais, personnellement, beaucoup aimé qu'il explique un peu mieux ce qu'elle fait effectivement, car elle laisse pas mal de questionnements aux lecteurs. :p
> 
> Pour le reste, disons que nous pourrions regretter qu'il n'explique pas que `std::endl` (car il devrait **toujours** utiliser le nom pleinement qualifier pour représenter les éléments de la bibliothèque standard, afin d'éviter au lecteur de croire que ça vient d'ailleurs) ne se contente pas de provoquer un retour à la ligne, mais qu'il provoque également un "flush" (la vidange complète) du tampon de la sortie standard.
> 
> Nous pourrions aussi regretter qu'il ne précise pas ce `std::cout` représente en réalité la sortie standard qui correspond, sur nos configurations classiques, le plus souvent la console, mais qui pourrait "tout aussi bien" correspondre à "autre chose" (comme je sais pas, moi, au hasard: une tablette braille).
> 
> Passons à la troisième page:
> 
> Lorsqu'il nous parle des noms (de variables), il nous dit
> 
> > si le nom se décompose en plusieurs mots, ceux-ci sont collés les uns aux autres
> 
> Il pourrait, au moins, préciser que l'on peut aussi utiliser l'underscore `_`, et qu'un nom proche de `la_poupee_qui_tousse` sera aussi correct que `laPoupeeQuiTousse` (en fonction de la convention de nommage utilisée).
> 
> Un peu plus loin, il nous donne une liste des types de données que l'on peut utiliser. Elle est "suffisante" dans un premier temps, mais clairement incomplète si on ne prend que les types primitifs en compte (il manque le type `long`, le type `long long`, les versions `unsigned` de tous les types entiers, le type `float` et le type `long double` pour qu'elle soit complète).
> 
> Pire encore, il place le type `std::string` dans la même liste, sans utiliser le nom pleinement qualifié, ce qui laisse croire à l'utilisateur que c'est le même genre de type de donnée que tous les autres de la liste. Ce qui est tout à fait faux !
> 
> Les types comme `bool`, `char`, `int` (et les versions `unsigned` équivalentes) ou `double` sont des types primitifs, que le compilateur C++ connaît d'office alors que `std::string` vient de la bibliothèque standard, et qu'il faut donc inclure un fichier d'en-tête spécifique pour que le compilateur en ait connaissance.
> 
> Vient ensuite l'initialisation des variable, et son fameux
> 
> ```c++
> TYPE NOM (VALEUR);
> ou
> TYPE NOM = VALEUR;
> ```
> 
> Le premier a été complètement abandonné depuis 2011 (cela fait quand même 9 ans !) et, si le deuxième est encore utilisé, nous préférerons désormais quand même toujours utiliser la syntaxe
> 
> ```c++	
> TYPE NOM {VALEUR};
> ```
> 
> Quant à son "astuce pour gagner de la place", qui consiste à regrouper la déclaration de plusieurs variables de même type sur une même ligne en les séparant par une virgule...
> 
> Bons dieux, mais quand est-ce que l'on arrêtera avec ces conneries????!!!!
> 
> - D'abord et avant tout, cela rend le code particulièrement difficile à lire, dans le sens où il est "beaucoup trop facile" de ne pas se rendre compte qu'une variable est déjà déclarée.
> - Ensuite, et surtout, la règle devrait **toujours** être de déclarer les variable au plus près de leur première utilisation, et, de préférence, lorsqu'il est possible  de leur donner une valeur directement utilisable par l'utilisateur.
> 
> Au moins, on peut être content pour une chose : il parle très rapidement des références. C'est peut-être le seul point positif de cette page ;) Passons donc à la suivante.
> 
> Sur la quatrième page, il nous parle de récupérer les informations introduites par l'utilisateur.
> 
> Il nous parle de `std::cin` et de `std::getline`, ce qui est déjà pas mal, mais il oublie de préciser **LA** chose, parmi les choses simples qui a sans doute provoqué le plus de problèmes, de crashes et de résultats aberrants dans les programmes : l'utilisateur est un imbécile distrait et on **ne peut pas** accorder **la moindre confiance** aux informations qu'il nous transmet.
> 
> Autrement dit, chaque fois que vous demandez à l'utilisateur d'introduire une donnée quelconque, vous devez vous assurer que ce qu'il aura introduit respecte les règles spécifiques devant être appliquées à cette donnée. Mais ça, il n'en parle nulle part.  Un peu comme s'il vivait dans le monde des bisounours, où tout le monde il est beau, tout le monde il est gentil.
> 
> Un peu plus bas, il va nous parler des constantes.  Mais il a déjà un train de retard, car il devrait en profiter pour parler des constantes de compilation (et en profiter pour expliquer la différence), `constexpr`, qui sont apparues depuis 9 ans, quand même.
> 
> Il faut attendre la cinquième page (de la version HTML) pour en trouver une sur laquelle il n'y a aucune remarque à faire (enfin, après une relecture rapide :p ).
> 
> Sur la sixième page, celle qui parle des fonctions, il nous présente la liste des argument comme étant
> 
> > les données avec lesquelles la fonction va travailler
> 
> C'est vrai… Mais ce n'est pas 
> 
> - Primo, parce que cela pourrait sous entendre qu'une fonction ne pourrait pas avoir de variables locales (ce qui est complètement faux).
> - Secundo parce que les arguments correspondent, surtout, aux données dont la fonction est incapable d'évaluer la valeur par elle-même
> 
> Ensuite, sa fonction toute simple qui prend la forme 
> 
> ```c++
> int ajouteDeux(int nombreRecu)
> {
>     int valeur(nombreRecu + 2);
>     return valeur;
> }
> ```
> 
> (j'ai supprimé les commentaires par facilité) est correcte, on peut difficilement dire le contraire…
> 
> Cependant, il laisse planer l'impression que les paramètres reçus par les fonctions seraient en quelques sortes des "données de second rang" qui ne peuvent pas être utilisées en tant que variables (quand ils ne sont pas constants) parce que nous serions obligés de déclarer une nouvelle donnée (valeur) pour représenter le résultat.
> 
> Or, un code proche de
> 
> ```c++
> int ajouterDeux(int valeur){
>     valeur+=2;
>     return valeur;
> }
> ```
> 
> serait tout à fait correct!  Et je ne parle même pas de présenter le sucre syntaxique (si cher à nos coeurs) qui nous permettrait d'écrire le code
> 
> ```c++
> int ajouterDeux(int valeur){
>     return valeur+2;
> }
> ```
> 
> De même, il présente le passage par référence constante (lorsque le paramètre est une `std::string`) comme
> 
> > une application bien pratique (...) Si votre chaîne de caractères contient un très long texte (la totalité de ce livre par exemple !)
> 
> Ce qui laisse entendre que, ben ... pfff ce n'est "pas si grave" de passer des `std::string` par valeur, hein?
> 
> **FAUX !** La copie occasionnée par le passage par valeur de n'importe quelle donnée "prenant d'avantage de mémoire qu'un type primitif" devrait toujours être évitée grâce au passage par référence (éventuellement constante). Quel que soit le type de la donnée, quelle que soit la valeur de la donnée transmise.
> 
> Mais bon, le reste de la page n'est "pas trop mauvais", du moins, pas au point de me faire dresser les cheveux sur la tête.
> 
> La septième page est dédiée à l'utilisation des tableaux... OUCHHH....
> 
> Il commence par nous présenter les tableaux de taille statique (définie à la compilation). Au moins, il n'oublie pas de préciser que leur taille doit toujours être une valeur constante, même s'il aurait pu le mettre plus en évidence.
> 
> Vient le moment où il nous parle de transmettre un tableau en paramètre à une fonction. Il nous dit explicitement que
> 
> > Il ne faut rien mettre entre les crochets.
> >
> > (...)Oui, je sais c'est ennuyeux. Mais il ne faut pas vous en prendre à moi, je n'ai pas créé le langage.
> 
> **FAUX !** Si on donne la taille d'un tableau (statique, dans le cadre qui nous intéresse), il faut juste veiller à ce que la taille soit une valeur constante. 
> 
> En plus, son exemple de fonction moyenne manque cruellement de *const-correctness*…
> 
> Il nous le dit lui-même:
> 
> > La deuxième restriction est qu'un tableau statique est **toujours passé par référence**
> 
> Ce qu'il oublie de dire, c'est que si on veut garantir que la fonction n'essayera pas d'aller modifier le contenu du tableau, il faut le dire explicitement au compilateur en transmettant le tableau sous une forme constante
> 
> Mais, au final, un code proche de
> 
> ```c++
> #include <iostream>
> 
> int const taille = 5;
> 
> float moyenne(const float notes[taille]){
>     float result{0};
>     for(int i{0};i<taille;++i)
>         result += notes[i];
>     return result/taille;
> }
> 
> void remplir(float notes[taille]){
>     for(int i=0;i<taille; ++i){
>         std::cout<<"Veuillez introduire la note "<<i+1<<" :";
>         float temp;
>         std::cin>>temp;
>         notes[i]=temp;
>     }
> }
> 
> void afficher(const float notes[taille]){
>     for(int i=0;i<taille; ++i){
>         std::cout<<"note "<<i+1<<" ="<<notes[i]<<"\n";
>     }
> }
> 
> int main(){
>     float notes[taille];
>     remplir(notes);
>     afficher(notes);
>     std::cout<<"la moyenne est de "<<moyenne(notes)<<"\n";
> }
> ```
> 
> compilera parfaitement et fournira le résultat qu'on lui demande, à savoir (par exemple)
> 
> ```text
> ./a.out
> # on introduit les notes
> Veuillez introduire la note 1 :19.5
> Veuillez introduire la note 2 :14
> Veuillez introduire la note 3 :6.5
> Veuillez introduire la note 4 :12
> Veuillez introduire la note 5 :11
> # on affiche les notes
> note 1 =19.5
> note 2 =14
> note 3 =6.5
> note 4 =12
> note 5 =11
> # on affiche la moyenne des notes
> la moyenne est de 12.6
> ```
> 
> On regrettera cependant qu'il n'ait pas pris la peine de parler de `std::array` que l'on préférera utiliser depuis C++11. ;)
> 
> Vient ensuite le tour de `std::vector`, dont on regrettera une fois de plus qu'il n'ait jamais mentionné le nom pleinement qualifié (mais bon, à ce stade, ce n'est qu'un détail).
> 
> La page suivante parle de l'utilisation des fichiers. 
> 
> On regrettera encore une fois qu'il n'utilise jamais les noms pleinement qualifiés `std::ifstream` et `std::ofstream`, mais bon, cela devient une habitude, n'est-ce pas?
> 
> On regrettera sans doute plus encore qu'il continue à utiliser la syntaxe pré 2011 lorsqu'il s'agit d'ouvrir un fichier dont le nom est donné par une `std::string`, à savoir
> 
> ```c++
> ostram monflux(nomFicher.c_str());
> ```
> 
> Parce que, avant 2011, il fallait effectivement transmettre le nom du fichier sous forme d'un `char *`.  Mais les choses ont changé depuis maintenant 9 ans ! On peut ouvrir un fichier en lui transmettant le nom du fichier à ouvrir sous la forme d'une `std::string`, à savoir
> 
> ```c++
> std::string nomFichier;
> /* ...*/
> ostram monflux(nomFicher);
> ```
> 
> Ce qui est quand même beaucoup plus facile. :D
> 
> Ah, au fait, tu as remarqué ? Je n'ai, jusqu'à présent, pas encore une fois cité la classe `std::exception` ! Mathieu non plus d'ailleurs. :P 
> 
> Or, arrivé à la huitième page du cours, les raisons pour lesquelles une exception pourrait te claquer à  la figure sont déjà très nombreuses. Ce serait pas mal d'être en mesure de les gérer dés maintenant, tu crois pas ?
> 
> On remarquera aussi qu'il n'a parlé ni des *range based loops* (les boucle prenant la forme de `for(element : tableau)`) , ni de l'inférence de type (le mot clé `auto`) qui facilitent pourtant tellement la vie du développeur. :p
> 
> En outre, il nous parle de `tellg()` / `tellp()` / `seekg()` / `seekp()`, un peu comme s'il était tout à fait normal de commencer à se balader dans notre fichier.
> 
> Il oublie juste de préciser que l'accès à un fichier -- que ce soit en lecture ou en écriture -- est typiquement le genre d'accès le plus lent (à l'exception du réseau) auquel nous pouvons être confronté sur un ordinateur.
> 
> Résultat des courses, il ne se passe pas deux mois sans qu'il n'y ait un débutant qui vienne poser une question sur le forum parce que "mon jeu est décidément très lent", alors qu'il passe son temps à se balader dans ses fichiers.
> 
> La neuvième page est le parfait exemple de tout ce que je viens de dire:
> 
> Passons sur l'usage de `srand()` et de `rand()`, qui sont des fonctionnalités issues du C et qui posent de sérieux problèmes, alors que nous préférerons les possibilités offertes (depuis 2011) par le fichier d'en-tête `<random>`, même si c'est aussi le genre de choses que l'on passe notre temps à répéter sur le forum.
> 
> Il y a tellement de discussions sur le forum concernant le TP du mot mystère que je ne vais pas revenir sur cette page. ;)
> 
> La dixième page a trait à une notion très importante (surtout en C): les pointeurs. Sauf que, en C++ moderne, elle ne devrait pas se trouver ici…
> 
> Ben oui, la notion de pointeurs ne devient très importante en C++ qu'à partir du moment où l'on commence à jouer avec des notions comme l'héritage ou le polymorphisme.  Or, à ce niveau-ci, on est encore très loin de le faire. :p
> 
> En plus, il nous fait jouer avec des `new` et des `delete`, mais il ne parle ni des pointeurs intelligent (dont `std::unique_ptr` en priorité), ni de la notion de propriété de la ressource pour laquelle on a eu recours à l'allocation dynamique. :p
> 
> Par la suite, on arrive dans la partie OO… Et on en a déjà tellement parlé que j'hésite à revenir dessus.

# Le naturisme des pointeurs et la dynamique allocation

Les pointeurs sont utiles en C++, ne nous mentons pas. Mais les pointeurs nus en C++, dans 99.5% des cas, c’est sale. Ce que j’appelle un pointeur nu ? C’est l’équivalent de *raw pointer* en anglais ; ce sont les pointeurs tels qu’on les voit en C. Mais qu’est-ce qui les rend sales ? **L’existence de moyens de s’en passer presque tout le temps**.

Déjà, il y a **les références**, qui nous simplifient la vie quand on fait un passage par argument, par exemple. Ensuite, il y a **les pointeurs intelligents** qui s’occupent pour nous de libérer la ressource acquise (parce qu'ils utilisent le merveilleux concept du [RAII](https://zestedesavoir.com/contenus/460/simplifier-la-gestion-de-la-memoire-en-c-avec-raii/). Enfin, **la bibliothèque standard**, avec tout ce qu’elle fournit, comme `std::vector` par exemple, nous permet de bannir les allocations dynamiques manuelles ; dans un langage à exceptions comme le C++, la gestion manuelle de la mémoire est un cauchemar aux vues du nombre possible de chemins d’exécutions différents.

Les allocations dynamiques n’ont d’ailleurs **pas leur place aussi tôt dans un cours**. Le débutant peine encore à jouer avec les fonctions et on devrait lui asséner un chapitre dessus ? Dans un cours de C, c’est déjà discutable, malgré l’importance de l’allocation dynamique. Mais dans un cours de C++, aux vues des arguments cités précédemment, **c’est une aberration**, ni plus ni moins ! Et non content d’utiliser `new` / `delete` sans vergogne, ce cours présente des code bogués. [Un exemple](https://openclassrooms.com/forum/sujet/erreur-heritage-poo#message-88917832).

# Des tableaux anciens

D’ailleurs, si liés aux pointeurs qu’ils sont en C, les tableaux entre crochets devraient être quasi-systématiquement **bannis d’un code C++ propre et moderne**. La bibliothèque standard nous fournit de nombreux conteneurs fiables, sécurisés et pratiques. Et qu’on ne vienne pas me dire que l’on ne peut pas avoir de tableau statique depuis que la norme C++11 a introduit `std::array` ! Donc les tableaux C, qu’ils soient statiques ou dynamiques, on a mieux en C++ donc on oublie.

# Les notions du fond de la caverne

Ou plus exactement, les trucs importants à savoir qui ne sont abordés qu’à la fin du tutoriel. Exemple ? **La gestion des erreurs**. C++ est un langage à exceptions. Celles-ci peuvent nous claquer dans les jambes mais on ne les voit que dans **un obscur chapitre d’une partie appelée « Notions avancées »**. Pourtant, rien n’est plus basique et la gestion des erreurs devrait faire partie des bases. Et ce n’est pas la seule tare de ce chapitre.

En effet, **il présente les exceptions là où n’elles n’ont pas de raison d’être** : les erreurs de programmation. Ainsi, si je passe un nombre négatif à une fonction calculant une racine carrée et qui attend un nombre positif, c’est une erreur de programmation et je suis le seul fautif. **Une assertion est beaucoup plus adaptée** : c’est la** programmation par contrats**.

Les exceptions sont à utiliser dans le cas de ressources à acquérir pouvant potentiellement échouer, comme se connecter à un serveur ou tenter d’ouvrir un fichier par exemple. Enfin, inutile d’en rajouter une couche sur **toute la bibliothèque standard qui est passée sous silence** et ne sort qu’à la partie IV du cours, alors qu’elle est tout simplement essentielle à un code de qualité.

Et quant à Qt, **pas un mot sur le système de gestion de mémoire parent/enfants**, qui est juste crucial pour faire un application Qt qui tient la route.

# Un cours du siècle passé

Il n’a **jamais été mis à jour pour C++11, C++14 ou C++17, **alors que** C++20 est déjà officiellement prête**. Lambda ? `constexpr` ? La *move-semantic* ? L’inférence de type ? Initialisation avec `{}` ? Inconnus au bataillon.

Autre exemple : le chapitre sur les fichiers, **totalement périmé**. Et ne parlons pas de Qt, **dont la partie n'a pas été mise à jour pour la version 5**, qui apporte notamment des changements profonds niveau connexion des slots et des signaux.

On peut aussi parler de l'utilisation de `srand` et `rand`, qui sont des reliques du C, alors qu'on dispose de [bien mieux](https://en.cppreference.com/w/cpp/numeric/random) depuis C++11.

# La conception bafouée et foulée aux pieds

Là, c’est le pire du pire, l’erreur qui ne pardonne pas et qui condamne ce cours comme étant **nul et à fuir**. Même les autres reproches ne sont rien face à ça. Prenons par exemple le cas du RPG, fil rouge de la partie II sur l’OO. Nous sommes bien d’accord qu’un personnage est quelque chose d’unique ; même si deux guerriers ont le même nom et un même type d’arme, ce sont néanmoins deux personnes distinctes. On parle de **sémantique d’entité**. Une classe à sémantique d’entité ne doit donc pas être copiée ni affectée, ce qui implique de supprimer le constructeur par recopie et `operator=()`.

Mais dans le cours d’OC, pas de soucis ! On peut le faire sans problème. C’est même utilisé dans le cours ([ici](https://openclassrooms.com/forum/sujet/erreur-heritage-poo#message-88917674) pour plus détails). Ce [long topic](https://openclassrooms.com/forum/sujet/comment-structurer-un-jeu-en-c) détaille d'ailleurs les inconvénients de cette méthode et comme mieux faire.

De plus, rien, **absolument rien sur comment bien concevoir un programme**. Je ne parle même pas des designs patterns (qui n’ont pas leur place ici) mais des règles de base, celles qui sont juste essentielles : SOLID et la loi de Demeter. Au contraire, [le livre de Philippe Dunski](https://www.d-booker.fr/programmation-et-langage/157-coder-efficacement.html) les aborde, même s’il est vrai que ce n’est pas un livre d’apprentissage du C++. Et franchement, **quel intérêt à enseigner à des débutants à programmer s’ils doivent tout désapprendre ensuite et suivre un autre cours pour enfin coder correctement** ? Voilà pourquoi je m’oppose au cours d’OC.

# Et pour quelques liens de plus

Parce que je ne suis pas le seul à le dire, et que ce n'est pas la première fois que le sujet est abordé sur les forums d'OC.

1.  [Début du cours C++](https://openclassrooms.com/forum/sujet/debut-du-cours-c#message-89384736), où @Bouli1515 cite quelques points qui sont repris dans ce post.
2.  [Programmer avec le langage C++](https://openclassrooms.com/forum/sujet/programmez-avec-le-langage-c-7#message-89251810)
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