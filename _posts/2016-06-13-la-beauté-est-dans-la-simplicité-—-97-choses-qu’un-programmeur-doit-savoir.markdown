---
layout: default
title: "La beauté est dans la simplicité — 97 choses qu’un programmeur doit savoir"
date: 2016-06-13 11:53:22
permalink: /:title/
---
Aujourd'hui, c'est le chapitre [La beauté est dans la simplicité](http://programmer.97things.oreilly.com/wiki/index.php/Beauty_Is_in_Simplicity) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par Jørn Ølmheim.

<!--excerpt-->

Il y a une citation dont je pense qu'il est bien que tous les développeurs connaissent et conservent près de leur cœur.

> La simplicité véritable allie la bonté à la beauté. — Platon

Cette seule phrase résumé les valeurs auxquelles nous, développeurs, aspirons. Il y a de nombreuses choses sur lesquelles se concentrent nos efforts dans notre code.

*   Lisibilité.
*   Maintenabilité.
*   Vitesse de développement.
*   L'évasive qualité qu'est la beauté.

Platon nous dit que le déclencheur de toutes ces qualités est **la simplicité**.

Qu'est-ce que du beau code ? C'est une question potentiellement très subjective. Ce que l'on perçoit comme beau dépend énormément du vécu individuel, tout autant que la perception que l'on a de ce qui nous entoure. Des gens élevés dans l'art auront une perception, ou tout du moins une approche, de la beauté différente de personnes bercées par les sciences. Les premiers décriront la beauté logicielle en comparant celui-ci à des œuvres d'art, quand les seconds parleront plutôt de symétrie et du nombre d'or, essayant de traduire les choses par des formules. De par mon expérience (NdT : celle de l'auteur, Jørn Ølmheim), la simplicité est la base de la majorité des arguments des deux camps.

Pense au code source que tu as étudié. Si tu n'as pas passé assez de temps à étudier le code des autres, arrête tout de suite de lire et file trouver un projet open-source à étudier. Sérieusement ! Je le pense vraiment ! Fais une recherche pour trouver du code dans le langage de ton choix, écris par des experts reconnus.

Tu es de retour ? Bien. Où étions-nous ? Ah oui ... J'ai trouvé que le code qui me fait vibrer et que je considère comme beau a un certain de propriétés en commun. La dominante, c'est la simplicité. Je pense que, peu importe la complexité de l'application ou du système, les parties prises séparément doivent rester simples. Des objets simples, avec une responsabilité unique, contenant des méthodes simples, précises, bien nommées. Certains pensent qu'avoir des méthodes courtes, de 5 ou 10 lignes, c'est un peu extrême ; certains langages rendent la chose difficile, mais je pense que cette concision est un objectif souhaitable.

La conclusion se résume à ce que **un code beau est un code simple**. Chaque partie reste simple quand ses responsabilités et ses couplages avec les autres parties restent simples. C'est ainsi que l'on peut conserver nos systèmes maintenables au fil du temps, avec un code propre, simple, testable, gardant une vitesse de développement élevée tout au long de la vie du système. La beauté naît de, et se trouve dans, la simplicité.

## Mon mot à moi

Qui ne rêve pas d'un système maintenable, clair, simple à comprendre, à faire évoluer ? Je suis d'accord avec l'idée que chercher la simplicité est une bonne chose, mais pas au point de descendre à 5 ou 10 lignes de code. Des fois, la simplicité de lecture ne vient-elle pas de quelques lignes en plus ?

Je pense à C# et [LINQ](https://en.wikipedia.org/wiki/Language_Integrated_Query) notamment. Je préfère parfois découper sur plusieurs lignes ou en plusieurs étapes une requête plutôt qu'une unique ligne de LINQ mais qui va demander des efforts pour comprendre. Dans ce cas, où est la simplicité, dans la première ou la deuxième façon de faire ?