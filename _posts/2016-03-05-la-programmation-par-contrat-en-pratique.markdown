---
layout: default
title: "La programmation par contrat en pratique"
date: 2016-03-05 17:31:27
permalink: /:title/
---
Suite à [l'article précédent](tdd-en-pratique) sur TDD, abordons la **programmation par contrat**, terme inventé par le créateur du langage Eiffel [Bertrand Meyer](https://fr.wikipedia.org/wiki/Bertrand_Meyer). Je sais bien qu'il existe de nombreux articles et publications sur le sujet. Je vais donc synthétiser tout ce que j'ai pu lire dans différents ouvrages (papiers ou en ligne) et illustrer le concept avec le projet des [Pages blanches](https://github.com/informaticienzero/Pages-blanches) introduit dans le post sur TDD.

<!--excerpt-->

## La programmation par contrat, qu'est ce que c'est ?

Une façon de programmer où l'on érige des règles à respecter par différentes parties qui signent, en quelque sorte, un contrat. Si les parties prenantes respectent ce dernier, alors on garantit le bon fonctionnement de l'objet du contrat.

En termes moins théoriques, l'utilisateur d'un morceau de code s'engage à **respecter certaines conditions** pour l'utiliser ; le code appelé s'engage lui aussi à **fournir le résultat attendu**. Les deux sont liés : si l'appelant respecte les conditions de l'appelé, il est en droit d'attendre des résultats corrects ; de même, si l'appelé voit ses conditions respecté, il doit tout faire pour répondre aux attentes de l'appelant.

Les conditions que l'appelant doit respecter sont appelées **les pré-conditions**. Celles que l'appelé doit garantir à la fin du traitement sont appelées **les post-conditions**. Celles qui doivent être garanties tout le long de l'exécution sont **les invariants**.

Illustrons avec un carré (exemple tiré de l'excellent livre ***Coder efficacement - Bonnes pratiques et erreurs à éviter (en C++)*** de ***Philippe Dunski***). Une pré-condition à la création d'un carré serait que l**a longueur des côtés ne soit pas négative ou nulle**. C'est au créateur du carré de vérifier ça. Une post-condition est que, pour une longueur valide, **l'aire du carré est bien égale au carré de la longueur**. Si ce n'est pas le cas, c'est l'implémentation, l'appelé, qui est en défaut. Enfin, un invariant serait qu'il y ait bien **quatre côtés de même longueur**. S'il y en a plus ou moins, ou s'ils ne sont pas tous exactement de la même longueur, ce n'est pas un carré.

## Les avantages de la PpC

*   Une **documentation vivante et à jour du code**. On sait, d'un rapide coup d’œil, les valeurs valides ou non que peut prendre la fonction, ce qu'elle retourne comme résultat ou comme erreur en cas de non-respect du contrat, les effets de bords éventuels en cas de rupture, etc. Et avec des langages comme D qui offre un support natif, ou Java qui permettent d'ajouter ces informations en générant le code, on gagne en puissance.

{% highlight d %}
class Date
{
	int day;
	int hour;

	invariant()
	{
		assert(1 <= day && day <= 31);
		assert(0 <= hour && hour < 24);
	}
}
{% endhighlight %}

{% highlight java %}
/**
 *  @inv !isEmpty() implies top() != null   //  no null objects are allowed
 */
public interface Stack
{
	/**
	 *  @pre o != null
	 *  @post !isEmpty()
	 *  @post top() == o
	 */
	void push(Object o);
	/**
	 *  @pre !isEmpty()
	 *  @post @return == top()@pre
	 */
	Object pop();
	/**
	 *  @pre !isEmpty()
	 */
	Object top();
	boolean isEmpty();
}
{% endhighlight %}


*Exemples tirés respectivement de la [page Wikipédia](https://en.wikipedia.org/wiki/Class_invariant#D) anglaise des invariants de classe et d'[un article sur iContract](http://www.javaworld.com/article/2074956/learn-java/icontract--design-by-contract-in-java.html).*

*   **On responsabilise les parties**. Je sais que l'utilisateur est bête. Mais à trop vouloir sécuriser ses entrées, on le déresponsabilise et il se dit que, peu importe ce qu'il passe, de toute façon l'appelé fera le nécessaire pour tester. Et bien non ! S'il ne respecte pas mes pré-conditions, pourquoi devrais-je respecter les post-conditions et tenter de rattraper ses erreurs ? C'est de sa faute.
*   **On améliore la qualité**. Le but d'un contrat est de garantir le bon fonctionnement d'un morceau de code si toutes les parties prenantes respectent leurs engagements. Ainsi, si un bug ou un comportement inhabituel est découvert, il suffit de regarder où le contrat à été rompu. Je pense, en écrivant cette phrase, à un système de logs qui notifierait là où rupture est apparue.

## Cas pratique

Si on regarde [le constructeur d'une page](https://github.com/informaticienzero/Pages-blanches/blob/master/Pages%20blanches/Page.cs), on constate que celui-ci ne prend qu'un argument qui est l'URL à charger. Or, comme le programme est spécifique aux pages blanches, il ne sert à rien d'essayer de vouloir charger une page autre. D'où les deux pré-conditions suivantes.

{% highlight java %}
// Si l'URL est null ou vide, c'est un erreur de programmation, donc assertion.
Contract.Requires(!string.IsNullOrEmpty(url), "URL is null or empty.");

// Si l'on n'est pas sur le site des pages blanches, ça n'a aucun intérêt de lancer le programme.
Contract.Requires(url.Contains("pagesjaunes.fr/pagesblanche"), "URL is not valid.");
{% endhighlight %}

Si l'utilisateur ne respecte pas ces conditions, alors je n'ai aucune raison de respecter le contrat moi non plus. De même, dans le cas du [découpage d'une ligne](https://github.com/informaticienzero/Pages-blanches/blob/master/Pages%20blanches/CSVAddressParser.cs), j'attends plusieurs choses, notamment que la ligne ne soit ni nulle ni vide et qu'elle contienne le bon nombre de champs. En retour, j'offre la garantie qu'il y a bien le bon nombre d'informations dans la Line que je renvoie.

{% highlight java %}
// Préconditions.
// Si la ligne à parser est null ou vide, c'est de la faute du développeur.
Contract.Requires(!string.IsNullOrEmpty(toParse), "String to parse is empty or null.");
// On vérifie que la chaîne à parser à bien les 15 points-virgules requis.
Contract.Requires(Regex.Matches(toParse, @"[;]").Count == CSVAddressParser.NumberOfRequiredSemiColon, "Number of ';' not expected. Should be " + CSVAddressParser.NumberOfRequiredSemiColon);

// Du code.

// Postconditions.
// On s'assure de bien avoir autant de ligne dans le dictionnaire que requis.
Contract.Ensures(dictionnary.Count == CSVAddressParser.InformationsPerLine);
{% endhighlight %}

## Des cas exceptionnels

Il ne faut pas tomber dans l'excès de mettre des assertions dans tous les sens. **Certaines erreurs ne sont la faute ni de l'appelant, ni de l'appelé**. Une connexion impossible à établir, une allocation qui échoue par manque de mémoire, un fichier manquant, tous ces cas sont exceptionnels. Là, mieux vaut privilégier l'utilisation des exceptions et leur possible rattrapage plutôt qu'une assertion qui claque bien sèchement.

## Que faire en cas de rupture ?

Si le contrat est rompu, plus rien n'est garanti. Si le programme n'est pas critique (sous-entendu si des vies ne sont pas en jeu), alors mieux vaut corriger celui-ci en analysant le code fautif. Si le contrat a été rompu, c'est qu'il y avait un problème dans des données, donc inutile de continuer à travailler avec celles-ci.

Je suis partisan, **dans le cas où une post-condition n'est pas remplie, d'envoyer une exception** afin de le signaler précisément à l'appelant. Pourquoi ? Il semblerait qu'on le préconise, comme [ici](http://julien-blanc.developpez.com/articles/cpp/Programmation_par_contrat_cplusplus/#LII-B-1) ou [ici](http://luchermitte.github.io/blog/2014/05/24/programmation-par-contrat-un-peu-de-theorie/#ii1--les-trois-contrats-de-la-ppc). L'idéal est d'envoyer une exception de runtime.

## Pour quelques liens de plus

J'ai essayé de regrouper le plus possible ce que je sais tout en étant clair. Mais comme la force d'internet c'est d'offrir plusieurs sources et plusieurs points de vue, voici des liens collectés au fil du temps.

*   Les pages Wikipédia [en français](https://fr.wikipedia.org/wiki/Programmation_par_contrat) et [en anglais](https://en.wikipedia.org/wiki/Design_by_contract) sur la PpC.
*   Une liste de trois articles par lmghs sur [la théorie](http://luchermitte.github.io/blog/2014/05/24/programmation-par-contrat-un-peu-de-theorie/), [les assertions ](http://luchermitte.github.io/blog/2014/05/28/programmation-par-contrat-les-assertions/)et [vue détaillée](http://luchermitte.github.io/blog/2015/09/17/programmation-par-contrat-snippets-pour-le-c-plus-plus/) de ce qu'on peut faire en matière de PpC avec C++.
*   Coder efficacement - Bonnes pratiques et erreurs à éviter (en C++) de *Philippe Dunski*.
*   Conception et Programmation Orientées Objet de *Bertrand Meyer*.
