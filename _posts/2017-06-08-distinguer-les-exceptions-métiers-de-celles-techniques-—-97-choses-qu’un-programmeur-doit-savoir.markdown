---
layout: default
title: "Distinguer les exceptions métiers de celles techniques — 97 choses qu’un programmeur doit savoir"
date: 2017-06-08 16:02:22
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Distinguer les exceptions métiers de celles techniques](http://programmer.97things.oreilly.com/wiki/index.php/Distinguish_Business_Exceptions_from_Technical) que je vais traduire, comme sa licence [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/) me le permet. Il a été écrit à l’origine par [Dan Bergh Johnsson](http://programmer.97things.oreilly.com/wiki/index.php/Dan_Bergh_Johnsson "Dan Bergh Johnsson").

<!--excerpt-->

Il y a, en gros, deux raisons pour lesquelles les choses tournent mal au runtime : **les problèmes techniques** qui nous empêchent d'utiliser l'application et **la logique business** qui nous empêche de mal l'utiliser. La plupart des langages, comme LISP, Java, Smalltalk ou C#, utilisent des exceptions pour remonter les deux situations. Cependant, elles sont différentes et doivent être séparées. C'est une potentielle source  de confusion que de les représenter toutes les deux par la même hiérarchie d'exceptions (NdT : cela semble se rapporter plutôt aux exceptions *checked* ou *unchecked* de Java), sans parler d'une même classe.

Un problème technique peut apparaître **à cause d'une erreur de programmation**. Par exemple, si tu tente d'accéder à l'élément 83 d'un tableau de taille 17, alors le programme déraille et une exception est remontée. Une version plus subtile consiste à appeler une bibliothèque avec des arguments incorrects, causant la même situation à l'intérieur de celle-ci.

Ce serait une erreur que de vouloir réparer une erreur qu'on a soi-même causé. Plutôt, on va laisser l'exception remonter jusqu'au niveau architectural le plus élevé et laisser un mécanisme général de gestion d'exceptions faire ce qui doit être fait pour maintenir le système dans un état stable, comme faire un roll back d'une requête, logguer, alerter un administrateur et le signaler (gentiment) à l'utilisateur.

Une autre variante, quand tu es dans la « situation de la bibliothèque » et qu'un appelant a rompu le contrat de ta méthode, par exemple en passant un argument bizarre ou en n'initialisant pas correctement un objet dépendant. C'est du même niveau que d'appeler le 83ème élément d'un tableau de taille 17 : l'appelant aurait dû vérifier ; ne pas l'avoir fait est une erreur de programmation côté client. La réponse appropriée consiste à lancer une exception technique.

Une autre situation, toujours technique, quand le programme ne peut fonctionner à cause d'un problème dans l'environnement d'exécution, comme une base de donnée qui ne répond plus. Dans ce cas, tu dois assumer que l'infrastructure a fait tout son possible pour résoudre le problème — réparer la connection et réessayer un nombre raisonnable de fois — et a échoué. Même si la cause est différente, la situation est la même pour l'appelant : il ne peut rien y faire. On signale donc celle-ci par une exception qui remontera jusqu'au mécanisme de rattrapage général.

À contrario, **il y a des situation où l'appel ne peut se compléter à cause d'une raison de logique métier**. Dans ce cas, nous rencontrons une situation exceptionnelle, c'est-à-dire inhabituelle et indésirable mais pas une erreur ou une bizarrerie programmatique, par exemple, si j'essaye de retirer de l'argent d'un compte n'ayant pas les fonds nécessaires. En d'autres termes, ce genre de situation fait partie du contrat, donc lancer une exception n'est qu'un chemin de retour alternatif qui fait partie du modèle et que le client doit connaître et prendre en compte. Pour ces cas, il est approprié de créer une exception spécifique, ou une hiérarchie d'exceptions séparée, afin que le client puisse gérer ça à sa façon.

Mélanger exceptions techniques et business, dans la même hiérarchie, brouille la distinction et l'appelant, à propos du contrat de la méthode, les conditions à remplir avant l'appel et quelles situations il est sensé gérer. Séparer les classes donne, au contraire, de **la clarté** et augmente les chances que les exceptions techniques soit gérées par un quelconque *application framework*, pendant que les exceptions business seront gérées par le code client.

## Mon mot à moi

Pas grand chose à dire, si ce n'est que j'essaye moi aussi de faire attention, en C#, dans le projet professionnel sur lequel je travaille, à séparer clairement les erreurs de fichier inexistant, de connexion à la base de donnée échouée, etc des exceptions métiers.