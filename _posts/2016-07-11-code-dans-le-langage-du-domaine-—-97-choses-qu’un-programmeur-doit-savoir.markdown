---
layout: default
title: "Code dans le langage du domaine — 97 choses qu’un programmeur doit savoir"
date: 2016-07-11 18:27:44
permalink: /:title/
---
Aujourd’hui, c’est le chapitre [Code dans le langage du domaine](http://programmer.97things.oreilly.com/wiki/index.php/Code_in_the_Language_of_the_Domain) que je vais traduire, comme sa licence [Creative Commons Attribution 3](http://creativecommons.org/licenses/by/3.0/us/ "http://creativecommons.org/licenses/by/3.0/us/") me le permet. Il a été écrit à l’origine par [Dan North](http://programmer.97things.oreilly.com/wiki/index.php/Dan_North).

<!--excerpt-->

Comparons deux bases de code. Dans la première, on tombe sur quelque chose comme ça :

{% highlight java %}
if (portfolioIdsByTraderId.get(trader.getId())
      .containsKey(portfolio.getId())) {...}
{% endhighlight %}

Tu te gratte la tête, tout en te demandant le but de ce code. Il semblerait qu'on obtienne l'ID d'un objet trader pour obtenir une map de, heu, une map de maps apparemment, pour cherche si un autre ID d'un objet portfolio existe dans cette précédente map. Tu te gratte encore plus la tête. Tu cherches la déclaration de portfolioIdsByTraderId et tu découvres ceci :

{% highlight java %}
Map<int, Map<int, int>> portfolioIdsByTraderId;
{% endhighlight %}

Petit à petit, tu comprends que cela à un rapport avec l'accès ou non d'un trader à un portfolio. Bien entendu, tu trouveras le même morceau de code — ou plutôt un morceau-similaire-mais-avec-quelques-subtiles-différentes — chaque fois qu'un morceau de code s'intéresse à l'accès d'un trader à un portefolio.

Dans l'autre base de code, tu tombe sur ceci :

{% highlight java %}
if (trader.canView(portfolio)) {...}
{% endhighlight %}

On ne se gratte pas la tête. Pas besoin de savoir comment un trader sait. Peut-être qu'il y a une de ces map de maps planquée quelque part à l'intérieur. Mais ça, c'est la tambouille du trader, par la notre.

Maintenant, dans quelle base de code voudrais-tu travailler ?

Il y a fort longtemps, nos structures de données étaient vraiment basiques : des bits, des bytes et des caractères (juste des bytes, mais disons qu'il y avait des lettres et de la ponctuation). Les décimaux étaient délicats parce que nos nombres base 10 ne marchent pas très bien en binaire, du coup, nous avions différentes tailles de types de flottants. Après sont apparus les tableaux et les chaînes de caractères (juste un type de tableaux différent). Puis se furent les piles, les files, les tables de hachage, les listes chaînées, les [listes à enjambements](https://fr.wikipedia.org/wiki/Skip-list) (NdT : les skip-list) et tout un tas d'autres structures de données excitantes *qui n'existent pas dans le monde réel*. L'informatique consistait à faire beaucoup d'efforts à transposer le monde réel dans nos structures de données restrictives. Les vrais gourous pouvaient même se rappeler comment ils l'avaient fait.

Puis vinrent les types définis par l'utilisateur ! Bon d'accord, ce n'est pas une nouvelle, mais ça change les règles du jeu. Si notre domaine contient des concepts comme des traders et des portfolios, on peut les modéliser avec des types appelés, disons, Trader et Portfolio. Mais, plus important, on peut modéliser *les relations entre eux* en utilisant les termes du domaine.

Si ton code n'utilise pas les termes, ou vocabulaire, du domaine, tu créés un accord tacite (comprendre : secret) expliquant que **ce** int ici est le moyen d'identifier un trader, tandis que **ce** int est un identifiant pour identifier un portfolio (mieux vaut ne pas les mélanger !). Et si tu représente un concept business (« Certains traders ne sont pas autorisés à voir certains portfolio — c'est illégal ») avec un fragment algorithmique, disons l'existence d'une relation dans un map de clés, tu ne fais pas de cadeau au gars chargé des audits.

Le programmeur suivant n'est peut-être pas dans la confidence, alors **pourquoi ne pas rendre ça explicite** ? Utiliser une clé pour chercher une autre clé qui va réaliser un test d'existence, ça ne coule pas vraiment de source. Comment quelqu'un est supposé deviner qu'ici sont implémentées des règles métier empêchant des conflits d'intérêt ?

Rendre le domaine explicite dans le code permet aux autre programmeurs de saisir **l'intention** du code bien plus facilement qu'en essayant de traduire l'algorithme en quelque chose de compréhensible vis-à-vis de ce qu'ils comprennent du domaine. Cela signifie également que quand le modèle métier change — ce qu'il va faire au fur et à mesure que ta connaissance du domaine grandit — tu es dans une bonne position pour faire évoluer le code. Couplé à une bonne encapsulation, les chances sont grandes que la règle n'existe qu'à un endroit, donc tu pourras le modifier facilement,

Le programmeur qui viendra quelques mois plus tard travailler sur le code te remerciera. Et ce programmeur pourrait bien être toi.

## Mon mot à moi

J'ai déjà appliqué ce principe sans m'en rendre compte dites-donc ! Dans notre base de code, plusieurs fois, nous avions un code qui accédait au membre du membre du membre d'une structure de données pour savoir si un code pin avait été rentré. J'ai encapsulé ce code dans une fonction nommée *CheckIfPinCodeEntered*, ce qui est quand même plus clair.

En plus, un code clair **facilite la relecture** et **réduit les commentaires à écrire**, ce qui réduit donc la possibilité qu'ils ne soient plus à jour.