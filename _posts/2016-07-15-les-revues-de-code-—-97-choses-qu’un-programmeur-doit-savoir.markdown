---
layout: default
title: "Les revues de code — 97 choses qu’un programmeur doit savoir"
date: 2016-07-15 12:00:34
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Les revues de code](http://programmer.97things.oreilly.com/wiki/index.php/Code_Reviews) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Mattias Karlsson](http://programmer.97things.oreilly.com/wiki/index.php/Mattias_Karlsson).

<!--excerpt-->

Tu devrais faire des revues de code. Pourquoi ? Parce qu'elle améliorent la qualité du code et réduisent le taux de défaut. Mais pas forcément pour les raisons auxquelles tu penses.

Parce qu'ils ont peut-être eu de mauvaises expériences avec les revues, de nombreux programmeurs ont tendance à ne pas les aimer. J'ai vu des organisations qui demandaient que tout le code soit formellement revu avant de passer en production. C'est souvent un architecte ou un lead dev qui fait cette revue, pratique que l'on peut résumer à **l'architecte revoit tout**. C'est inscrit dans leurs livres sur le processus de développement logiciel, alors les programmeurs doivent s'y soumettre. Il y a sans doute des organisations qui ont besoin d'un tel cadre rigide et formel, mais la plupart non. En fait, pour beaucoup, une telle approche est contre-productive. Ceux qui sont revus peuvent se sentir jugés par une commission de remise en liberté conditionnelle (NdT : longue phrase traduisant *a parole board*). Ceux qui revoient ont besoin de temps pour lire le code mais aussi pour se tenir à jour par rapport à tous les détails du système. Les revues peuvent rapidement devenir le goulet d'étranglement et le processus dégénérer.

Au leu d'une simple correction du code, le but des revues de code devrait être de **partager la connaissance** et d'établir des lignes directrices communes de codage. Partager ton code avec les autres programmeurs permet son appropriation collective. Laisse un quelconque membre parcourir le code avec le reste de l'équipe. A la place d'une recherche d'erreurs, il faut revoir le code en essayant de l'apprendre et le comprendre.

**Sois gentil** durant les revues de code. Assures-toi que les commentaires sont constructifs, non acerbes. Introduis différents rôles de revue pendant cette réunion, pour empêcher que la revue ne soit affectée par une ancienneté organisationnelle présente dans l'équipe. Des exemples de rôles seraient un membre focalisé sur la documentation, un autre sur les exceptions et un troisième sur les fonctionnalités. Cette approche aide à la répartition de la charge à travers l'équipe.

**Mets en place une journée par semaine** consacrée à la revue de code. Passe deux ou trois heures dans une réunion spécialement consacrée. Change la personne revue à chaque réunion avec une simple rotation. Rappelle-toi d'échanger les rôles également. **Implique les débutants** dans ces revues. Ils sont peut-être inexpérimentés, mais leurs connaissances universitaires toutes fraiches peuvent fournir un autre point de vue. **Implique les experts** pour leur expérience et leurs connaissances. Ils identifieront les codes douteux plus rapidement et avec plus de précision. Les revues de code se passeront mieux si l'équipe a des conventions de codage vérifiées par outils. De cette manière, le formatage du code ne sera jamais abordé pendant la revue.

**Rendre les revues de code amusantes** est certainement la facteur de succès le plus important. Les réunions de revues dépendent des personnes impliquées. Si elles sont une plaie ou ternes, il sera dur de motiver les intéressés. Transforme-la en revue de code informelle dont le but principal est le partage des connaissances à travers les membres de l'équipe. Laisse les commentaires sarcastiques à l'extérieur et ramène plutôt un gâteau ou un [*brown bag lunch*](http://linsolas.github.io/blog/2013/02/09/lancez-vous-dans-les-brown-bag-lunches/).

## Mon mot à moi

Les revues de code sont importantes, à n'en pas douter. Partager la connaissance rend l'équipe responsable collectivement du code et répartit la connaissance, évitant ainsi de reposer sur un super-héros, connaissant tout mais dont l'absence pénalise l'équipe. Là encore, on est en plein dans le ***software craftmanship*** et la volonté de faire du bon travail et d'améliorer collectivement le code.

Le [TechTrends](http://www.xebia.fr/livresblancs.html) de Xebia sur le sujet propose deux pratiques pour le partage de connaissances, en plus des revues de code « classiques ». Je cite :

> *
> 
> Les **Brown Bag Lunches** (BBL). Ces sessions, au format conférence ou table ronde, voient généralement la présence d’un speaker désigné, interne ou externe à l’entreprise, qui présentera, pendant 1 à 2 heures, un sujet qu’il maîtrise. Le format BBL favorise les échanges et les participants doivent ressortir de ces conférences avec une vision d’ensemble de la technologie présentée et, éventuellement, avoir identifié des champs d’application au sein de l’entreprise et de leurs projets. Ce format permet une discussion “universitaire” mais aussi de profiter des retours de terrain.
> 
> *
> 
> Les **coding dojos**. C’est en forgeant qu’on devient forgeron. L’adage est transposable au monde du génie logiciel. C’est en pratiquant le code que le jeune développeur s’aguerrira et deviendra “maître” développeur. Cela n’est pas toujours possible sur un projet, car il est impossible d’explorer au quotidien toutes les facettes du métier de développeur. Les séances de coding-dojo sont un moyen pour contourner ces problèmes : un groupe de personnes s’entraîne en binômes, sur un même problème durant 30 minutes. À la fin du temps imparti, le code est effacé, puis on recommence à coder avec un autre partenaire de travail. Le but est ainsi de se perfectionner et de découvrir de nouvelles techniques de conception ou de programmation. Ce simple moment de partage de connaissances peut s’avérer très bénéfique en marge d’un projet.

Pour les revues de code, [Octo](http://blog.octo.com/revue-de-code-quel-format-choisir/) propose deux formats : la revue collective et la revue par un pair. En effet, bien que [Mattias](http://programmer.97things.oreilly.com/wiki/index.php/Mattias_Karlsson) propose des revues collectives, celles-ci ne sont pas toujours possibles, car plus contraignantes à mettre en place qu'une revue à deux. Il ne faut pas négliger cette dernière, parce qu'elle est tout à fait complémentaire à la forme collective. Je cite le blog d'Octo.

> On peut aussi utiliser les revues de code collectives formelles en complément des revues en paires :
> 
> *
> 
> en format court restreint aux points qui ne sont pas évidents, non couverts par les standards.
> 
> *
> 
> sur certains algorithmes ou éléments de design complexes.
> 
> *
> 
> pendant le période de mise en place des revues, afin que l’équipe se forme collectivement à la pratique.

Je n'ai que (trop) rarement été participant à une revue collective, mais je suis déjà content qu'on fasse des revues par pair régulièrement à mon travail. J'aime le partage de connaissances et l'idéologie du *software craftmanship*. Je vous invite à lire le [TechTrends](http://www.xebia.fr/livresblancs.html) de Xebia sur ce thème, c'est gratuit et instructif.