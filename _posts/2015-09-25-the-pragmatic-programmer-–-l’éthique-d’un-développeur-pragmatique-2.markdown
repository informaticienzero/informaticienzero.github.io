---
layout: default
title: "The Pragmatic Programmer – L’éthique d’un développeur pragmatique 2"
date: 2015-09-25 22:49:46
permalink: /:title/
---
Dans la [première partie](2015-08-17-the-pragmatic-programmer---l'éthique-d'un-développeur-pragmatique-1.markdown) de cet article, nous avons abordé plusieurs points. Sous la métaphore d'un chat mangeur de code source, nous avons compris qu'**un bon programmeur ne fournit pas de mauvaises excuses mais plutôt qu’il doit fournir des solutions**. D'ailleurs, c'est lui qui prend en main sa carrière, ses projets et son travail quotidien, même si cela lui demande **humilité** et **modestie**.

Ensuite, nous avons examiné l'importance de corriger la moindre vitre brisée dans le projet. Ne pas le faire peut couler un projet si bien parti. D'ailleurs, depuis la publication de la première partie de cet article, un [commitstrip](http://www.commitstrip.com/fr) est sorti, s'amusant d'un problème pas si éloigné.

![](http://www.commitstrip.com/wp-content/uploads/2015/09/Strip-Dilemme-de-codeur-7-650-final1.jpg)

Enfin, nous nous sommes quittés sur l'histoire de la soupe de cailloux et la grenouille, pour comprendre **l'importance d'être un catalyseur** amenant des changements positifs tout en évitant de **trop se laisser absorber par les détails** au point de perdre l'image générale du projet.

Il est donc temps d'examiner les 3 autres principes, qui concluront ce premier chapitre.

# Des logiciels de qualité

Aux États-Unis existe une blague mettant en scène une compagnie américaine achetant 100.000 circuits intégrés à un fabriquant japonais. Une des conditions était de n'avoir qu'une pièce défecteuse par lot de 10.000. Quand, quelques semaines plus tard, l'entreprise américaine reçu sa commande, elle remarqua qu'il y avait deux paquets. L'un contenait des milliers de composants ; l'autre, minuscule, n'en contenanait que 10 avec joint un message disant « *Voici les pièces défectueuses.* »

Une telle qualité est malheureusement bien compliquée à obtenir dans le monde du développement. Les développeurs ont des contraintes de temps, de technologie, de mentalité aussi (certains pourraient se dire « *Pourquoi se casser la tête à essayer de faire un excellent produit ?* »). Mais ce constat ne nous laisse pas sans espoir : **il est possible de créer des logiciels suffisamment bons**.

Cette section du premier chapitre de *The Pragmatic Programmer*, contrairement à ce qu'on pourrait penser, ne vas pas parler de la qualité du code (bonne conception, tests unitaires, preuve formelle, etc) mais va plutôt parler de la **qualité du produit lui-même**. Répond-il aux besoins ? Correspond-il aux attentes du client ? Est-il agréable à utiliser ?

Le livre suggère par exemple d'**impliquer tous les utilisateurs et acteurs sur la qualité du produit**. On peut faire le lien aujourd'hui avec les méthodes agiles et les délais très courts entre chaque itération qui permet au client d'avoir un retour presque en temps réel sur le produit. L'ajustement par rapport à ses attentes et besoins sera ainsi plus facile et rapide.

Dans le genre d'écueils à éviter, il y a ceux qui ignorent complètement les utilisateurs ou les contraintes (du projet, de temps, d'argent, etc) simplement pour ajouter une fonctionnalité supplémentaire ou peaufiner le code à l'excès. À l'autre opposé, c'est tout aussi peu professionnel de passer outre les bases de la conception et les bonnes pratiques pour tenter de tenir des délais impossibles. Après, ces écueils sont-ils imputables uniquement aux développeurs ? Non, il faut aussi que les autres membres du projet (architectes, chefs de projets, commerciaux) jouent le jeu en ne demandant l'impossible ni au client ni aux équipes de développement. Mais là, on tombe dans un autre domaine, celui de la gestion de projet.

La morale de cette section, résumée en une phrase est de **faire de la qualité du produit une fonctionnalité requise** aussi importante que les aspects techniques.

Cette section conclut avec un piège dans lequel il est facile de tomber par excès de zèle : **le perfectionnisme**. Le perfectionnisme, sous prétexte de vouloir encore plus améliorer, peaufiner, corriger, au final, ne livre jamais rien si l'on ne le force. Et puis, qui ne préfère pas avoir un bon logiciel aujourd'hui qu'un logiciel parfait demain ?

*<troll>Voilà pourquoi je suis sous Archlinux et non Debian.</troll>*

# Un portefeuille de connaissances

> Un investissement dans la connaissance rapporte toujours les meilleurs intérêts.
> 
> - Benjamin Franklin

Je n'ai pas l'habitude de noter les citations du livre, mais celle-ci m'a paru excellente. En effet, est-ce que la connaissance et l'expérience ne sont pas les aspects les plus précieux et les plus importants pour un développeur ? Et puis, sans investissement, on stagne, nous ne sommes plus à jour, nos connaissances peuvent se périmer (exemple de C++ pré et post 2011), nous perdons de la valeur au fil du temps qui passe. Investir dans la connaissance est donc important. Commençons donc par voir 5 conseils donné dans le livre.

1.  **Investir régulièrement**, qu'investir devienne une habitude. Même si la somme est petite. Tout investissement est bon à prendre.
2.  **Diversifier**. C'est la clé du succès à long terme. De plus, la diversification apporte de la valeur : plus on élargie ses connaissances et plus l'on est à même de s'adapter aux situations ou au changement. Pour illustrer, un topic a été créé [sur Zeste de Savoir](https://zestedesavoir.com/forums/sujet/3990/langage-fonctionnel/) à propos des langages fonctionnels ; beaucoup d'intervenants conseillaient l'apprentissage d'un de ces langages parce qu'il aide à mieux réfléchir et utiliser les langages impératifs ou objets.
3.  **Sécuriser ses investissements**. Un investisseur raisonnable ne va pas placer tout son argent dans des placements à hauts risques, même si ceux-ci peuvent rapporter beaucoup plus. Mais à l'opposé, il ne va non plus tout mettre dans des placements sécurisés mais particulièrement peu rentables. De même, un bon développeur doit apprendre à trouver l'équilibre entre des technologies émergeantes qui peuvent disparaitres du jour au lendemain et des technologies matures mais bien ancrées.
4.  **Acheter à bas prix, revendre très cher**. Dans la suite directe du point ci-dessus. Apprendre une technologie émergeante est parfois une véritable gageure, à cause du manque de documentation, des bugs, du peu de personnes capables de répondre à des questions qu'on se pose, etc. Mais qu'elle devienne populaire et murisse et bing ! le gros lot ! À l'heure où j'écris cet article (24 septembre 2015), je pense à [Docker](https://www.docker.com/), une technologie basée sur les conteneurs Linux dont on entend de plus en plus parler depuis 2014 et qui grimpe. Bien qu'apprendre à bien s'en servir semble encore un peu compliqué, je pense que ça vaudra son beau pesant d'or d'ici quelques années, quand Docker sera mature et répandu.
5.  **Mettre à jour et rééquilibrer son portefeuille**. Un bon exemple est C++. On peut avoir investi dedans avant 2011 et enrichi son portefeuille avec. Mette ce dernier à jour consistera à apprendre les nouveautés depuis cette date.

La suite du chapitre contient plusieurs conseils d'investissements. Je ne détaillerai que ceux que je tente de mettre en pratique.

*   **Apprendre au moins un nouveau langage par an**. Ça, à l'université, je l'ai fais sans même avoir lu le livre. En 2010, j'ai commencé par le C. J'ai commencé le C++ en 2012, Java en 2013, HTML / CSS en 2014, PHP, JavaScript, Python et OCaml en 2015. Mais bon, puis-je vraiment dire que j'ai appris des langages à l'université vu la vitesse à laquelle on les a survolés. Disons qu'en 2015, mes efforts portent et vont porter sur Python et OCaml. Et je suis chanceux, il y a d'excellentes ressources francophones. Pas de raison pour ne pas que j'y arrive.
*   **Lire un livre technique chaque trimestre**. Hum non, là je peux pas. Si *The Pragmatic Programmer* peut se lire sans problème en un trimestre, le prochain sur la liste, *Conception et Programmation Orientées Objet*, qui doit faire dans les 1500 pages et qui est bien technique, je ne pourrais jamais en faire une lecture de qualité en un trimestre. Dîtes donc Dave et Andy ! j'ai une vie à côté !
*   **Livre un livre non technique chaque trimestre**. Bein tiens, moi qui parlait d'avoir une vie à côté. J'aime beaucoup lire, donc cet objectif est assez facile à remplir. TPP nous dit qu'il est important de lire ce genre de livres pour comprendre et s'adapter à toutes ces personnes pour qui un ordinateur est une grosse boîte noire. J'aimerais bien lire des livres sur le marketing, l'économie et les entreprises pour ça. Sinon, dans les lectures sans but précis, j'aime bien les livres sur le monde ferroviaire.
*   **Varier les environnements et les technologies**. C'est pas trop dur, je suis sous GNU/Linux et toutes les personnes autours de moi (sauf les possesseurs de machines Apple et quelques très rares personnes aussi sous le pingouin) tournent sous Windows. Alors autant dire qu'à ce niveau là, je varie. J'essaye aussi de varier en programmation avec différents langages différents (C++, Python, OCaml). Ah si, je varie en mettant à jour mon Galaxy S III avec la dernière version d'Android qui sort (au moment où j'écris ces mots, je tourne avec Android 5.1).
*   **Rester connecter**. Je fais partie de [Zeste de Savoir](https://zestedesavoir.com/), un super site pronant le partage de connaissances gratuitement et librement. En plus des nombreux tutoriels informatique, on peut aussi en apprendre sur la géologie, la psychologie, le fonctionnement du cerveau, les mathématiques et de très nombreuses autres thématiques. En plus de rester connecté au monde de l'informatique, je m'ouvre aux autres savoirs. J'ai presque l'impression de faire tous les points précédents d'un coup tiens !

Mais si apprendre est bien, **il ne faut pas donner crédit à tout ce qu'on lit, entend ou voit**. N'oublions pas que l'on vit dans un monde rempli d'entreprises et de corporations qui veulent vendre leurs produits, même en informatique. Ce n'est pas parce qu'une page Internet est la première référencée par un moteur de recherche qu'elle est forcément de qualité ou objective.

Ça me rappelle des articles dans le magazine *Programmez* qui présentent un outil résolvant tel ou tel problème, le dit article étant écrit par la société éditrice de l'outil. De la publicité quoi. Tout n'ést pas à jeter, mais il faut néanmoins **garder un esprit critique** en lisant ce genre de publication.

# Communique !

Un développeur communique tout le temps. Dans des réunions avec le client pour comprendre ses besoins (tiens, on me souffle dans l'oreillette que dans un monde parfait ce ne serait pas au développeur de faire ça). On écrit du code grâce à un langage, signe de notre communication avec la machine. On documente aussi, pour nous-mêmes mais aussi les développeurs futurs. On échange avec son équipe pour définir l'orientation du projet, on défend ses idées, on propose des améliorations. Et tiré de mon expérience professionnelle : on râle après la machine à café. En bref, **il faut apprendre à bien communiquer**.

*   **Savoir quoi dire**. Nous ne sommes pas des écrivains, nous voulons être clairs mais concis. TPP préconise pour cela d'écrire un plan avec les idées que l'on veut développer. Cette technique est valable tant pour les écrits que pour des présentations devant public ou des appels. Croyez-en mon expérience, écrire un plan rend vraiment le discours plus clair et plus naturel.
*   **Connaître son auditoire**. Il faut savoir quelles sont leurs connaissances, leurs besoins et leurs intérêts. Parler de langage de programmation ou de framework n'aura pas de sens pour un public de commerciaux. Andy et Dave propose un acronyme regroupant plusieurs questions à se poser : **WISDOM **(sagesse en anglais).

    *   **W**hat do you want them to learn ? Que veux-tu qu'ils apprennent ?
    *   What is their **i**nterest in what you've got to say ? Quel intérêt ont-ils à t'écouter ?
    *   How **s**ophisticated are they ? À quel point sont-ils spécialistes ?
    *   How much **d**etail do they want ? Quel niveau de détail veulent-ils ?
    *   Whom do you want to **o**wn the information ? Parmi eux, qui doit s'approprier l'information ?
    *   How can you **m**otivate them to listen to you ? Comment les motiver à t'écouter ?

*   **Choisir le bon moment**. Mais aussi s'adapter aux priorités de ses interlocuteurs. Le livre donne l'exemple d'un responsable qui vient de se faire incendier par sa supérieure parrce que le code source a disparu, expliquant que ce peut être le moment idéal pour lui parler des gestionnaires de code source (quel sadisme n'empêche).
*   **Adapter son style**. Traduction un peu maladroite, mais l'idée est de changer la forme en fonction de la personne. On se doute bien qu'on ne parlera pas de la même façon à ses co-développeurs que devant le plus gros client de la boite. De même, certaines personnes peuvent, pour un moment X, attendre un gros rapport quand d'autres se contenteront d'un mémo ou d'un mail. Le but est de communiquer pour s'adapter.
*   **Soigner la forme**. Là, on parle de productions écrites. À l'heure du *material design *(personnellement, je trouve ça clair, fluide et sans fioriture), de LaTeX, des logiciels de traitements de texte puissants, il n'existe plus d'excuse pour ne pas produire des documents soignés et propres, d'autant plus que beaucoup d'entreprises ont déjà des templates. Mais soigner la forme veut dire aussi soigner la langue, l'othographe, la grammaire, la conjugaison (bon là dessus, je serai gentil, je suis pas le mieux placé).
*   **Impliquer son auditoire**. Lui communiquer régulièrement les brouillons du document. Cela peut grandement aider à l'améliorer.
*   **Écouter**. La communication doit être dans les deux sens ; dans le cas contraire, c'est un monologue. Même quand on a toutes les informations, si l'on n'écoute pas les autres, ils ne seront pas enclins à nous écouter. Pour les aider à parler, on peut leur demander de résumer ce qui vient d'être dit, de les questionner pour répondre à d'éventuelles interrogations.
*   **Répondre**. Personnellement, je déteste les gens qui ne répondent pas. Mais loin de leur jetter la pierre, puisque il m'arrive de faire de même. Et je ne pense pas être le seul. TPP propose d'envoyer un simple message signalant à la personne concernée qu'on ne l'oublie pas mais qu'on revient vers elle dès que possible.

Nous voici arrivés à la fin de ce premier chapitre, chapitre présentant un peu l'éthique d'un développeur pragmatique. Dans le chapitre deux, nous verrons comment avoir cette fois une **approche pragmatique**. Mais en attendant, bonne méditations de ces principes et bonne mise en pratique !
