---
layout: default
title: "L'agencement du code compte — 97 choses qu’un programmeur doit savoir"
date: 2016-07-14 13:12:21
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [L'agencement du code compte](http://programmer.97things.oreilly.com/wiki/index.php/Code_Layout_Matters) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Steve Freeman](http://programmer.97things.oreilly.com/wiki/index.php/Steve_Freeman).

<!--excerpt-->

Il y a un nombre incalculable d'années, je travaillais avec un système écrit en Cobol où nous n'étions pas autorisé à modifier l'indentation si nous n'avions pas une raison valable de changer le code, simplement parce qu'un jour, quelqu'un a cassé quelque chose en laissant une ligne se retrouver dans les colonnes spéciales au commencement d'une ligne. Cette règle s'appliquait même si l'agencement était trompeur, ce qui était parfois le cas, ce qui nous forçait à lire le code très attentivement puisque nous ne pouvions pas lui faire confiance. Cette mesure a du coûter cher en renouvellement de programmeurs.

Il y a eu des recherches prouvant que l'on passe beaucoup plus de temps à naviguer dans le code pour le lire — pour trouver où faire les changements — qu'à en écrire, d'où une volonté d'optimiser ça.

- **Facile à lire**. Nous sommes très bons pour reconnaître motifs et patterns, donc je me fais une faveur en fondant dans le décor, par standardisation, tout ce qui n'est pas lié au domaine, toute la complexité accidentelle accompagnant la majorité des langages de programmation. Si du code qui agit pareil ressemble à un autre code, alors mon système perceptif m'aidera à trouver les différences. C'est également pour ça que j'observe des conventions dans l'agencement des parties d'une classe dans une unité de compilation : constantes, champs, méthodes publiques, méthodes privées.

- **Un agencement expressif**. Nous avons tous appris à prendre du temps pour trouver les bons identificateurs pour que le code exprime aussi clairement que possible ce qu'il fait, plutôt que simplement énumérer les étapes, n'est-ce-pas ? L'agencement du code fait partie de cette expressivité. Un premier pas consiste à ce que l'équipe soit d'accord sur un outil de formatage automatique, puis on pourra faire quelques ajustements à la main pendant que je code. A moins d'une grosse dissension, l'équipe va rapidement converger vers un style "à la main" commun. Un formateur ne peut pas comprendre mes intentions et il m'est plus important que les découpages et regroupements des lignes reflètent les intentions du code, non la syntaxe du langage.

- **Un format compact**. Plus je peux avoir d'informations sur mon écran, plus je peux voir sans rompre le contexte en scrollant ou en changeant de fichiers, actions qui impliquent que je garde moins d'informations dans ma tête. Des commentaires de fonctions très longs et une montagne d'espaces avaient du sens pour des identifiants à 8 caractères et des *line printers*, mais maintenant je vis avec un IDE qui offre la coloration syntaxique et du *cross-linking*. Mon facteur limitant, ce sont les pixels, donc je veux que chacun d'eux contribue à ma compréhension du code. Je veux que l'agencement m'aide à comprendre le code, mais pas plus que ça.

Un ami non-développeur m'a fait remarqué un jour que le code ressemble à de la poésie. J'ai ce sentiment devant un code vraiment bon, où tout à un but et m'aide à comprendre l'idée. Malheureusement, écrire du code n'est pas aussi romantique, dans l'imaginaire, que d'écrire de la poésie.

## Mon mot à moi

Les outils de formatage automatiques, mouais. Je ne suis pas convaincu. Si c'est pour repasser après toutes les 5 minutes, très peu pour moi. Par contre, un bon IDE avec de la coloration syntaxique de qualité, quel plaisir à regarder. Je pense à Visual Studio quand je fais du C#. C'est de toute bôôôté !