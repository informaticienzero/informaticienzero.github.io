---
layout: default
title: "Le code est la conception — 97 choses qu’un programmeur doit savoir"
date: 2016-07-12 10:30:16
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Le code est le design](http://programmer.97things.oreilly.com/wiki/index.php/Code_Is_Design) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Ryan Brush](http://programmer.97things.oreilly.com/wiki/index.php/Ryan_Brush).

<!--excerpt-->

Imagine que demain, en te réveillant, tu apprends que l’industrie du bâtiment a fait la découverte du siècle. Des millions de robots, peu chers et incroyablement rapides, peuvent fabriquer des matériaux de nulle part, en ayant une coût énergétique proche de zéro tout en se réparant eux-mêmes. Et mieux : en fournissant un plan non ambigu d’un projet de construction, les robots peuvent le bâtir sans intervention humain, le tout à un coût négligeable.

On peut imaginer l’impact que cela aurait sur l’industrie du bâtiment, mais que ce passerait-il en amont ? A quel point le comportement des architectes et des designers changerait si les coûts de construction étaient négligeables ? Aujourd’hui, des modèles physiques et informatiques sont créés et rigoureusement testés avant de lancer la construction. Est-ce qu’on s’en soucierait si la construction était quasi-gratuite ? Si un design s’effondre, pas de soucis — il suffit de trouver ce qui a échoué et laisser nos robots magiques en construire un autre. Il y a d’autres implications. Avec des modèles obsolètes, les designs inachevés évoluent par construction et amélioration répétées sur une approximation du projet final. Un observateur non-averti aurait du mal à distinguer un design inachevé d’un produit fini.

Notre capacité à prédire les délais disparaîtrait. Les coûts de construction se calculent beaucoup plus facilement que les coûts de design — on connaît le coût approximatif pour installer une poutre, ainsi que le nombre de poutres dont on a besoin. Alors que le nombre de tâches prédictibles diminue jusqu’à zéro, les temps de design, de moins en moins prévisibles, dominent.

Bien entendu, les pressions liés à une économie concurrentielle se font sentir. Avec les coûts de construction éliminés, une compagnie qui peut rapidement créer des designs gagne un avantage sur le marché. Faire les choses rapidement devient un leitmotiv des sociétés d'ingénierie. Inévitablement, quelqu’un manquant d’expérience avec la conception verra une version non validée, réfléchira à l’avantage sur le marché de sortir le produit tôt et dira « Ça me semble suffisamment bon. »

Certains projets de vie ou de mort seront plus diligents, mais dans beaucoup de cas, **les clients apprendront à souffrir de cette conception inachevée**. Les entreprises peuvent toujours envoyer leurs robots magiques réparer les bâtiments ou véhicules vendus. Tout ceci nous amène à une conclusion étonnante et contre-intuitive : notre postulat de base était une simple réduction substantielle des coûts de construction, mais nous n’avons obtenu qu’**une baisse de la qualité**.

Cela ne surprendra personne de savoir que cette histoire s’est jouée dans le monde logiciel. Si l’on accepte que le code est la conception — un processus créatif et non mécanique — la **crise logicielle** s’explique. Nous avons actuellement une **crise de conception*** *: la demande pour de la qualité et des designs validés dépasse largement nos capacités à les créer. La tentation d’utiliser des designs inachevés est grande.

Heureusement, ce modèle offre également des indices sur comment améliorer la situation. Les simulations physiques correspondent aux tests unitaires ; la conception d’un logiciel n’est jamais complète tant qu’elle n’est pas validée par une batterie de tests de malade. Pour rendre ces tests plus efficaces, nous faisons tout pour ne pas nous enfoncer dans le vaste monde des grands systèmes logiciels, gros et lourds (NdT : la phrase d’origine est « *To make such tests more effective we are finding ways to rein in the huge state space of large systems* »). Des langages de qualité et des bonnes pratiques de conception nous donnent un espoir. Enfin, il y a une vérité implacable : **les grands designs font faits par de grands designers se consacrant à la maîtrise de leur métier**. Le code, c’est la même chose.

## Mon mot à moi

Là, on est clairement dans le **mouvement du software craftmanship**. On cherche la qualité de la conception et du code, tout en refusant les solutions mal conçues, à court terme et les compromis sur la qualité. Et cette histoire que nous conte [Ryan](http://programmer.97things.oreilly.com/wiki/index.php/Ryan_Brush) est tout à fait vraie. Combien de projets brouillons, mal écrits car vite faits pour prouver ou vérifier des choix (les fameux PoC) deviennent finalement des projets à temps plein tournant en production ? Même dans des projets définis comme pour la production dès le départ, il y a cette dette technique, ces fixs à l’arrache, ce mauvais code qu’on corrigera plus tard.

Je suis particulièrement d’accord avec le fait que du bon code est produit par **quelqu’un de passionné** qui se consacre à son art. Définir ce qu’est un bon code est un exercice compliqué, voire périlleux, auquel je ne me prêterai pas. Mais de mon point de vue, si l’expérience professionnelle compte, un vrai passionné, **codant en dehors du travail**, se remettant en question, toujours prompt à se former, cette personne a plus de chance de faire des erreurs, donc d’apprendre de celles-ci pour ne plus les reproduire et augmenter la qualité de ce qu’il écrit.
