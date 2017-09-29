---
layout: default
title: "Mon amour de C# (adieu Java)"
date: 2016-03-19 00:47:19
permalink: /:title/
---
J'utilise C# tous les jours au travail. Bon d'accord, la version 3 avec Visual Studio 2008 et le framework .NET 3.5, ce qui donne plus l'impression de faire de l'archéologie que de la programmation. Mais j'ai aussi Visual Studio 2015, Community certes, mais très bien pour faire tout un tas de tests ou de projets open-sources. En plus, comme ils ne concernent pas mon entreprise, je ne tombe pas sous la limitation à 250 PC ou 1 million de chiffre d'affaire. Bref. Tout ça pour dire que **plus j'utilise C# et plus je l'apprécie** et ce **beaucoup plus que Java**.

<!--excerpt-->

## Quand est-ce que tu as pratiqué Java ?

À l'université. C'est avec ce langage que j'ai eu l'introduction à la programmation et à la conception orientée objet, ainsi que le développement Android. J'ai eu l'occasion de le pratiquer un petit peu en dehors, mais sans jamais faire appel à des mécanismes ou des spécificités de Java, mes projets étant de simples réécritures de code C++.

Je n'aime pas vraiment ce langage, notamment certains des choix de conceptions ou l'absence de mécanismes présents en C++ et/ou en C#. Je vais vous expliquer pourquoi.

## Les properties

Il arrive régulièrement d'offrir des services d'accès à des données internes à nos objets. On les appelle les **getters** et les **setters**. Typiquement, pour une classe Chat, obtenir le nom du chat. Je ne vais pas m'attarder sur l'utilité et la justification des getters (simple à démontrer) et des setters (beaucoup plus dur à justifier dans beaucoup de cas), je réserve ça à un éventuel futur article.

Non, prenons un exemple très bête comme le nom d'une personne. Voilà comment je dois faire en Java.

{% highlight java %}
public class Person
{
	private String name;

	public String getName()
	{
		return this.name;
	}

	public void setName(String other)
	{
		this.name = name;
	}
}

// ...

Person p = new Person();
p.setName("Gloved girl doing Java");
System.out.println("Name : " + p.getName());
{% endhighlight %}


**C'est lourd**, surtout quand le nombre de champs dont on ouvre l'accès se multiplie. Par contre, depuis C#3, les properties permettent de faire un code aussi simple que celui-là.

{% highlight csharp %}
public class Person
{
	public string Name { get; set; }
}
// ...
Person p = new Person();
p.Name = "Gloved girl doing C#";
Console.WriteLine("Name : " + p.Name);
{% endhighlight %}


Et voilà le travail. Le compilateur va se charger de générer le reste, il est grand et peut se débrouiller tout seul. Bien entendu, rien ne m'empêche de modifier l'accessibilité de mes accesseurs ou de modifier l'implémentation par défaut.

{% highlight csharp %}
public class Person
{
	// Le setter est privé et limité à l'usage de la classe.
	public string Name { get; private set; }

	// Setter non déclaré, membre en lecture seule.
	public int Age { get; }
}
{% endhighlight %}

C'est quand même sacrément plus clair et plus agréable à utiliser. Un exemple concret ? La classe [List(T)](https://msdn.microsoft.com/fr-fr/library/6sh2ey19(v=vs.110).aspx) qui offre comme service de connaître le nombre d'éléments qu'elle contient avec la propriété `Count`.  Ou bien encore dans la classe [People](https://github.com/informaticienzero/Pages-blanches/blob/master/Pages%20blanches/People.cs) de mon projet de page blanche, pour récupérer et/ou modifier le nom ou l'adresse d'une personne.

Soyons honnêtes : **ce n'est pas spécifique à C#**. D'autres langages comme D, Python ou Ruby le font. Mais pas Java. Donc voilà, sur ce point, je préfère C# à Java.

## Visual Studio

Un IDE excellent. Il n'y a pas d'autre mot. Déjà, **le thème sombre**. Eh oui, c'est tout bête mais ça fait un bien fou aux yeux. En plus, je code avec [Ryū](https://www.google.fr/search?q=Ryu&safe=active&source=lnms&tbm=isch&sa=X&ved=0ahUKEwiP-KaIpMvLAhUGPxoKHfSxA5sQ_AUIBygB&biw=1366&bih=656) et ça, c'est la classe qui déchire !

D'un côté plus pratique, il y a **IntelliSense**, l'autocomplétion absolument géniale, rapide et permettant d'utiliser pleinement .NET. Exemple ? Dans un fichier vierge de tout `using`, il suffit de faire `IList<>` pour qu'on se voit proposer d'ajouter `using System.Collections.Generic;` ou de faire `System.Collections.Generic.IList`. Je trouve ça vraiment impressionnant. De tous les IDE C++ que j'ai utilisé, je n'ai jamais vu ça. Avec Java et Eclipse, je dois avouer que je ne sais pas du tout, j'ai oublié comment c'était.

Il y a aussi le **gestionnaire de packets NuGet**, l'intégration de **Github** et** TFS**, la **génération automatique de code** comme les properties ou les delegates, les options de **refactoring, **sa** vitesse** (qui n'a jamais pesté face à lenteur bureaucratique d'Eclipse ?), la possibilité d'**utiliser d'autres langages** plus facilement qu'avec Eclipse (avis vraiment personnel pour le coup), comme Python, installable en même temps et tout aussi simplement que C# ou C++. Et je pense que j'oublie d'autres choses encore qui me plaisent.

## La surcharge d'opérateurs

Venant de C++, surcharger des opérateurs est quelque chose de tout à fait normal pour moi et est totalement justifié dans le cas de classes à [sémentique de valeur](http://cpp.developpez.com/faq/cpp/?page=Les-classes-en-Cplusplus#Quand-est-ce-qu-une-classe-a-une-semantique-de-valeur), telles qu'une classe représentant des nombres complexes ou des polynômes. Et franchement, la méthode Java de les interdire pour forcer à utiliser des méthodes, j'aime pas.

{% highlight java %}
Complex lhs = new Complex(1.0, 3.0);
Complex rhs = new Complex(2.0, 5.0);

Complex answer = lhs.add(rhs);       // add two complex numbers
		answer = lhs.subtract(rhs);  // subtract two complex numbers
{% endhighlight %}

À comparer avec C# qui permet la surcharge d'opérateurs.

{% highlight csharp %}
Complex lhs = new Complex(1.0, 3.0);
Complex rhs = new Complex(2.0, 5.0);

Complex answer = lhs + rhs;       // add two complex numbers
		answer = lhs - rhs;  // subtract two complex numbers
{% endhighlight %}

En plus, pourquoi Java interdit-il la surcharge d'opérateurs ? Est-ce par peur que les développeurs fassent n'importe quoi en utilisant des opérateurs pour faire des opérations qui n'ont rien à voir, comme ce qu'on reproche à C++ avec les fluxs ? **C'est stupide**. Le programmeur doit prendre ses responsabilités et assumer ses choix. Sinon, on bannit presque tout et on ne l'autorise qu'à créer des entiers.

Et puis, quand je fais `System.out.println("Bob number " + 42);` , c'est pas de la surcharge implicite de l'opérateur + ça ?

### Les choix des concepteurs

Mes recherches sur [StackOverflow](http://stackoverflow.com/questions/77718/why-doesnt-java-offer-operator-overloading) semblent indiquer que c'est effectivement un choix personnel du concepteur, James Gosling.

> I left out operator overloading as a **fairly personal choice** because I had seen too many people abuse it in C++.
> 
> [Source](http://www.gotw.ca/publications/c_family_interview.htm)

À l'opposé, voici les propos du père du C++.

> Many C++ design decisions have their roots in my dislike for forcing people to do things in some particular way [...] Often, I was tempted to outlaw a feature I personally disliked, I refrained from doing so because **I did not think I had the right to force my views on others**.
> 
> *Bjarne Stroustrup. Source: The Desing and Evolution of C++ (1.3 General Background)*

D'accord d'accord, je m'égare. Mais j'aime vraiment pas cette limitation de Java. Pas du tout.

## Lancer des exceptions (paf ! dans les dents)

C# est un langage à exceptions, comme Java et comme C++. Mais Java, c'est vraiment une plaie enfoncée profondément dans le pieds. **Je ne supporte vraiment pas de devoir préciser, dans le prototype d'une fonction, quelles exceptions seront lancées**. Je parle de ceci.

{% highlight java %}
public void doA() throws Exception1, Exception2 {
{% endhighlight %}

Les exceptions qui seront potentiellement lancées sont déjà visibles dans le code, pourquoi créer de la redondance en les rajoutant encore une fois dans le prototype ? Et comme on lit plus souvent la documentation d'une fonction que son corps, pourquoi ne pas faire comme C# et embarquer les exceptions dans la documentation XML de la fonction ?

Franchement, quand je vois la méthode [File.ReadAllLines](https://msdn.microsoft.com/fr-fr/library/s2tte0y1(v=vs.110).aspx) en C# et toutes les exceptions potentiellement claquables, je me dis que je suis content que C# n'ai pas eu la mauvaise idée de copier Java sur ce points. Sinon on aurait le droit à quelque chose de ce genre.

{% highlight csharp %}
public static string[] ReadAllLines() throws ArgumentException, ArgumentNullException, PathTooLongException, DirectoryNotFoundException, IOException, UnauthorizedAccessException, FileNotFoundException, NotSupportedException, SecurityException
{% endhighlight %}

## Object et nullables

En C#, tout dérive de la classe `Object`. Oui, tout, mêmes les types valeurs comme `int`, `string` ou `bool`. Pas comme Java. Cet état de fait autorise des constructions moins lourdes pour les nullables en C# qu'en Java, en plus d'être plus cohérent.

{% highlight java %}
Optional<Boolean> loveJava = null;
{% endhighlight %}

{% highlight csharp %}
bool? loveCs = null;
{% endhighlight %}

Bon, j'avoue que je triche parce que le code C# présenté est une abréviation de l'écriture suivante.

{% highlight csharp %}
Nullable<bool> loveCs = null;
{% endhighlight %}

Mais au moins, on peut utiliser le mot-clef `bool` auquel on est habitué et non devoir passer par ce truc batard `Boolean`.

## Les méthodes d'extension

C'est un mécanisme permettant, en quelques sortes, d'**ajouter des méthodes à des types existants sans les modifier ni les dériver**. Je ne vais pas détailler le comment du fonctionnement, juste donner un exemple avec mon projet de [pages blanches](https://github.com/informaticienzero/Pages-blanches/blob/master/Pages%20blanches/Extensions.cs). Plusieurs objets retournent des `IList<>` mais ces interfaces ne définissent pas de méthode concaténant deux listes. Pas grave, pas un problème pour moi.

{% highlight csharp %}
using System.Collections.Generic;

namespace PagesBlanches
{
	public static class Extensions
	{
		/// <summary>
		/// Méthode d'extension utilisée pour concaténer des IList.
		/// </summary>
		/// <typeparam name="T">Le type des objets à énumérer.</typeparam>
		/// <param name="collection">Une collection d'objets T.</param>
		/// <param name="enumerable">Une énumération d'objets T</param>
		public static void AddRange<T>(this IList<T> collection, IEnumerable<T> enumerable)
		{
			foreach (var current in enumerable)
			{
				collection.Add(current);
			}
		}
	}
}
{% endhighlight %}

Et voilà ! maintenant, je peux ajouter n'importe quel objet implémentant l'interface `IEnumerable<T>` à une `IList<T>`, sans avoir eu accès au code de cette dernière ni sans dérivation quelconque. Puissant, n'est-il pas ?

## Abrégeons la torture

J'ai beaucoup tapé sur Java et comme tu l'as bien compris, **je préfère largement C#**. Mon avis n'est pas le plus objectif ni le plus impartial qu'on ait vu. J'avoue que j'ai été dégoûté de Java assez vite, donc je n'ai pas poussé au-delà de mes années d'université. D'ailleurs, je pense que je ne pousserai plus jamais. Non, j'aime pas le développement Android.

À l'opposé, **je suis vraiment content de C#**. Je pense que je vais m'orienter vers ce langage dans le cadre professionnel et continuer à travailler avec.
