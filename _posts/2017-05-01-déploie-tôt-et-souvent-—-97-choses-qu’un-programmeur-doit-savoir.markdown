---
layout: default
title: "Déploie tôt et souvent — 97 choses qu’un programmeur doit savoir"
date: 2017-05-01 07:30:53
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Déploie tôt et souvent](http://programmer.97things.oreilly.com/wiki/index.php/Deploy_Early_and_Often) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Steve Berczuk](http://programmer.97things.oreilly.com/wiki/index.php/Steve_Berczuk "Steve Berczuk").

<!--excerpt-->

Le débogage des processus de déploiement et d'installation est souvent repoussé jusqu'à la toute fin du projet. Dans certains projets, l'écriture d'outils d'installation est délégué à un release engineer, qui considère la tâche comme un « mal nécessaire ». Les revues et les démos sont faîtes depuis un environnement fabriqué à la main pour s'assurer que tout fonctionne. Le résultat, c'est que **l'équipe n'acquiert aucune expérience sur le processus de déploiement** ou sur l'environnement de déploiement, jusqu'à ce qu'il soit peut-être trop tard pour opérer des changements.

Le processus d'installation / de déploiement est la première chose qu'un client voit, et un processus simple est la première étape pour avoir un environnement de production fiable (ou, tout du moins, facilement débogable). L'outil de déploiement est ce que le client utilisera. En ne s'assurant pas que le déploiement initialise l'application correctement, cela éveillera des interrogations chez le client, avant même qu'il n'ait commencé à utiliser l'application.

Commencer le projet avec un processus d'installation va te donner le temps de le faire évoluer au fur et à mesure que tu avances dans le cycle de développement du produit, ainsi que la chance de pouvoir modifier le code pour rendre l'installation plus simple. Lancer et tester le processus d'installation sur un environnement vierge, périodiquement, fournit l'assurance que **le code ne fait aucune hypothèse basée sur l'environnement de développement ou de test**.

Faire passer le déploiement en dernier veut dire que le processus peut être plus compliqué que nécessaire pour contourner ces hypothèses dans le code. Ce qui semblait être une idée brillante dans l'IDE, quand tu avais tout le contrôle sur l'environnement, peut en fait grandement complexifier le processus de déploiement. Mieux vaut connaître tous les compromis tôt que tard.

Même si être capable de déployer ne semble avoir que peu de valeur business face au fait de voir l'application tourner sur le poste d'un développeur, la vérité, c'est que, jusqu'à ce que tu puisse démontrer le bon fonctionnement de l'application sur l'environnement cible, il y a beaucoup de travail à faire pour délivrer de la valeur business. Si la raison pour laquelle tu repousse la création de ce processus est que ça te semble facile, alors **autant le faire de suite**. Si c'est trop compliqué, ou s'il y a trop d'incertitudes, fais ce que tu ferais avec le code de l'application : **expérimente, évalue et refactorise** le processus de déploiement en avançant.

Le processus d'installation / de déploiement est essentiel pour la productivité de tes clients ou des équipes métiers, donc tu dois le tester et le refactoriser tout en avançant. Nous le faisons pour le code tout au long d'un projet ; le déploiement n'en mérite pas moins.

## Mon mot à moi

C'est amusant de traduire cet article, puisque je viens d'achever [un gros refactoring](refactoring-de-code-legacy) sur le processus de génération de l'application PDA à mon travail justement. Alors j'ai relativement de la chance puisqu'un processus automatisé existait déjà, mais il avait besoin d'un bon coup de ménage pour devenir un outil plus rapide et plus facile à utiliser. Par contre, d'autres équipes n'en n'ont pas…

D'ailleurs, de nombreux [outils DevOps](https://xebialabs.com/periodic-table-of-devops-tools/) sont dans cet optique. Je pense notamment à [Docker](https://www.docker.com/), qui permet de conteneuriser une application et donner des garanties fortes sur le système utilisé, sa version, les versions des dépendances, etc. Également [Ansible](https://www.ansible.com), qui permet d'automatiser la création d'environnement et supprime le besoin de taper les commandes à la main, tout en offrant un versionning des environnements si on l'intègre au gestionnaire de code source. Ou bien encore [Vagrant](https://www.vagrantup.com/). On peut par exemple utiliser Ansible pour créer un environnement, installer les dépendances nécessaires et lancer automatiquement le build d'un conteneur Docker.
