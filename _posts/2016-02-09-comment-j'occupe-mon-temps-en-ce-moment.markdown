---
layout: default
title: "Comment j'occupe mon temps en ce moment"
date: 2016-02-09 22:20:57
permalink: /:title/
---
J'ai plus de temps que d'habitude à consacrer au développement et à l'informatique, alors autant en profiter pour avancer sur des sujets qui me plaisent. Et comme j'ai découvert plusieurs choses que je trouve vraiment bien, je profite du billet d'aujourd'hui pour vous les faire découvrir.

<!--excerpt-->

# CodeCombat, programmer en jouant

C'est en parcourant la page profil d'un membre de CodinGame que j'ai découvert cette nouvelle plate-forme, [CodeCombat](http://codecombat.com/). Le principe est excellent : **s'entraîner à programmer concrètement**, puisque les instructions que l'on écrit servent à déplacer le personnage, faire qu'il tape sur des vilains pas beaux ou d'autres actions. On démarre avec du très basique comme les variables ou les conditions, puis on va en augmentant le niveau.

En plus, le projet est [open-source](https://github.com/codecombat/codecombat) et traduit en français. On ne peut même pas s'excuser en prétendant que l'anglais c'est trop dur ! Si ça vous intéresse, voilà [mon profil](http://codecombat.com/user/informaticienzero), reconnaissable entre mille. :)

Après, comme la plate-forme vise quand même un public jeune et qui ne s'y connait pas vraiment, il n'y a que **peu de langages proposés** et tous assez proches : Python, JavaScript, CoffeeScript, Clojure, Lua. Si je suis très content qu'il y ait Python car c'est une excellente façon de l'apprendre, il est dommage qu'on ne propose pas d'autres langages pour ceux qui connaissent déjà et ne veulent que s'amuser. Je n'aime pas non plus le fait que l'on ait accès à certains niveaux qu'en payant $9.99 par mois.

# Les livres blancs de Xebia

Xebia, cabinet de conseil international, a édité plusieurs [livres blancs](http://blog.xebia.fr/tag/techtrends/). Étant curieux d'en apprendre un peu plus sur tout ce qui revient le plus souvent dans la sphère informatique du moment, c'est à dire tout ce qui est agilité, Big Data, DevOps, Data Labs, etc, quand j'ai vu les différents thèmes, **j'ai commandé plusieurs livres** : sur l'agilité, sur DevOps, sur le back et le front, sur le Cloud, sur Big Data et les Data Labs et enfin sur Craftsmanshift.

J'ai déjà pu lire ce dernier. J'ai trouvé le sujet intéressant et c'était la première fois que j'entendais parler de l'artisanat du code. Lisant moi-même **The Pragmatic Programmer**, on sent que le mouvement craftsmanship en est inspiré. L'idée est que **la qualité du code est toute aussi importante que la qualité métier** du logiciel. L'accent est donc mis sur les bonnes pratiques, la revue de code, le *Test-Driven Development*, etc, afin de fournir un code évolutif, clair, fiable, dont la maintenance est facilitée.

Les qualités d'un craftmanship sont les suivantes.

*   **Qualité** (conception simple, tests, TDD).
*   **Humilité** (je me remets en question et je m’améliore en continu).
*   **Partage** (je partage mes acquis avec mes pairs, je m’enrichis chemin faisant).
*   **Pragmatisme** (je n’applique pas bêtement, je comprends ce que je fais et je m’adapte si nécessaire).
*   **Professionnalisme** (je traite mon client comme un partenaire, je sais dire non quand nécessaire).
*   **Concret** (pas de certifications fumeuses, mais un retour aux techniques fondamentales : OO, [SOLID](https://fr.wikipedia.org/wiki/SOLID_(informatique)), [DDD](https://en.wikipedia.org/wiki/Domain-driven_design)).

Le livre n'est bien entendu qu'une introduction, pas un guide complet. Il n'empêche que je l'ai trouvé intéressant et qu'on devrait sans doute le mettre dans toutes les mains, avec l'utopie de voir un maximum de développeurs s'attacher à fournir du code de qualité.

J'ai aussi fini aujourd'hui **celui sur les Data Labs**. Bien que non concerné par Big Data dans mon travail, j'étais intrigué par le titre et titillé par l'envie de démystifier un peu ce terme ressorti à toutes les sauces. Ce livre explique ainsi l'intérêt de mettre en place **des moyens de profiter des masses colossales de données offertes par Big Data**, puis comment développer l'idée, se concentrer sur les développements à valeurs métier les plus fortes, convaincre les différentes parties (marketing, métier, SI, etc) de l'intérêt de cette structure, les PoC, les éventuelles pérennisations des développements, etc. Bien que court car simple introduction en la matière, je suis resté quelque peu sur ma faim. Je suis certes un peu moins dans le brouillard, mais toujours relativement perdu. N'empêche que sa lecture était intéressante.

Les livres sont disponibles en ligne ou à l'envoi, gratuitement. N'hésitez pas à jeter un œil pour vous faire votre avis.

# Cours en ligne sur Docker

Ça fait longtemps que je n'y ai pas touché, mais j'ai toujours envie d'apprendre à utiliser cet outil. Ainsi, en tapant sur Google les mots « *docker devops* », je suis tombé sur [ce cours](https://www.udemy.com/the-docker-for-devops-course-from-development-to-production/) proposé par [Udemy](https://www.udemy.com/), dont le montant s'élève à 56€. Je ne connais rien à cette plate-forme de cours en ligne. Reste que les avis sur le cours ont tout l'air d'être positifs ; d'autres sujets proposés me plaisent aussi, comme [Learn Devops: Continuously Deliver Better Software](https://www.udemy.com/learn-devops-continuously-deliver-better-software/) à 35€ ou [Jenkins Bootcamp: Fully Automate Builds Through Deployment](https://www.udemy.com/jenkins-continuous-integration-bootcamp/) à 56€. Bien entendu, je ne pourrai jamais me payer tous les cours qui m'intéressent, mais je pense que ceux-là valent le coup.

# Les Clash of Code

J'ai découvert ça hier. Le principe est simple : **des défis face à d'autres joueurs**, solvables en 5, 10 ou 20 min, avec difficulté croissante. Je n'ai testé que les 5 minutes, appelés *Flash*, et je me suis bien amusé ! Les exercices sont assez simples, mais le fait d'être limité en temps mets un peu la pression et force à se souvenir de ce qu'on sait plutôt que de sauter sur son moteur de recherche favori.

Le plus amusant : certains défis consistent à **coder pour résoudre un énoncé masqué,** avec pour seuls indices les jeux de tests. Il faut alors bien lire les tests fournis pour voir le rapport existant entre les entrées et les sorties. Par exemple, avec les couples "Code | 2", "Bonjour | 3", "krp | 0", "eau minérale | 7", on comprend que le programme doit compter le nombre de voyelles d'une chaîne de caractères.

Ces petits jeux permettent de bien s'échauffer en se jetant dans le bain direct, à 9h le matin. Rien de tel pour réveiller son cerveau et le préparer à la journée de codage qui l'attend !
