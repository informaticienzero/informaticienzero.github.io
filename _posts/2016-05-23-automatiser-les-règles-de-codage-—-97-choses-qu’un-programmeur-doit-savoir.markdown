---
layout: default
title: "Automatiser les règles de codage — 97 choses qu’un programmeur doit savoir"
date: 2016-05-23 19:55:44
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Automatiser les règles de codage](http://programmer.97things.oreilly.com/wiki/index.php/Automate_Your_Coding_Standard) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Filip van Laenen](http://programmer.97things.oreilly.com/wiki/index.php/Filip_van_Laenen).

<!--excerpt-->

Tu dois connaître ça. Au commencement d'un projet, tout le monde est bourré de bonnes intentions - appelle ça des « résolutions de nouveaux projets ». Bien souvent, beaucoup d'entre elles sont inscrites dans des documents, ceux dédiés aux règles de codage du projet. Lors de la réunion de démarrage, le lead developer parcourt le document et dans le meilleur des cas, tout le monde est d'accord pour suivre ces règles. Mais une fois que le projet est lancé, **ces bonnes intentions sont abandonnées**, une par une. Quand le projet aboutit et est livré, le code ressemble à un beau boxon et personne ne comprend comment on a pu en arriver là.

Quand est-ce que les choses ont dérapé ? Sans doute dès la réunion de démarrage. Certains membres ne faisaient pas attention. D'autres n'ont pas compris. Pire, il y en a même qui n'étaient pas d'accord et prévoyaient déjà leur rébellion contre ces normes de codage pour imposer les leurs. Enfin, certains avaient bien compris et étaient d'accord, mais la pression était telle qu'ils ont du laisser tomber quelque chose. Eh oui, un code bien formaté ne fait pas gagner de points avec un client qui attend plus de fonctionnalités. De plus, suivre un standard peut être vraiment ennuyant si ce n'est pas automatisé. Essaye donc d'indenter une classe bordélique à la main si tu ne me crois pas.

Si c'est un tel problème, pourquoi s'obstiner à vouloir des standards d'abord ? Une raison qui pousse à formater le code de manière uniforme est d'empêcher que quelqu'un ne « s'approprie » un morceau de code en le formatant de la façon qu'il lui convient. On veut aussi empêcher les développeurs d'appliquer des anti-patterns, dans le but d'éviter certains bugs courants. En fait, un standard de code devrait rendre le travail sur le projet plus facile et aider à maintenir la vitesse de développement constante du début à la fin. Cela va sans dire que tout le monde doit être d'accord avec ces règles - ça ne sert à rien si un développeur utilise trois espaces pour indenter alors qu'un autre en utilise quatre.

Il existe une abondance d'outils utilisables pour produire des rapports sur la qualité du code, ainsi que pour documenter et maintenir le standard, même toute la solution ne réside pas ici. Tout cela devrait être automatisé et imposé chaque fois que possible. Voici quelques exemples.

*   Faire en sorte que **le formatage du code soit partie intégrante du build**, de manière à ce que n'importe qui puisse le lancer en même temps qu'une compilation.
*   Utilise des **outils statiques d'analyse de code** pour chercher les anti-patterns et pour empêcher le build le cas échéant.
*   Apprends à configurer ces outils pour **scanner des anti-patterns** spécifiques au projet.
*   Ne te contente pas de mesurer la couverture de code par les tests mais également les résultats de ces derniers. Si la couverture est trop faible, le build doit échouer.

Fais ceci pour chaque chose que tu considères comme importante. Tu ne pourras pas automatiser tout ce qui te tiens à cœur. Pour tout ce que rentre dans ce cas, considère que ce sont des règles complémentaires au standard automatisé ; accepte également que toi et tes collègues ne les suiviez pas avec autant de soin.

Enfin, le standard doit être dynamique et non statique. En même temps que le projet évolue, les besoins du projet changent et ce qui était une bonne décision pour le début du projet n'est peut-être plus nécessaire quelques mois après.

## Mon mot à moi

J'ai déjà eu l'occasion d'utiliser [SonarQube](http://www.sonarqube.org/) mais pas encore de façon assez sérieuse. C'est pourtant une bonne idée puisque le plugin pour Python est gratuit. Je ferai un retour dessus dès que je l'aurai utiliser sur un « vrai » projet.
