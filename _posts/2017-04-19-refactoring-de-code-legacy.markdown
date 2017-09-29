---
layout: default
title: "Refactoring de code legacy"
date: 2017-04-19 12:52:33
permalink: /:title/
---
Je n'ai pas écris d'article depuis la semaine dernière parce que j'ai profité d'un sprint plus léger pour **lancer un refactoring** d'une partie de l'application Windows Mobile du boulot, refactoring qui était vraiment nécessaire. Cela m'a pris la semaine pour écrire un code plus clair, plus robuste et plus rapide. Ça n'a pas été évident, mais le résultat en vaut la peine, tant pour l'équipe qui aura un meilleur outil que pour moi qui a acquit de l'expérience. Je ne vais pas faire tout un topo sur [ce qu'est le refactoring](https://en.wikipedia.org/wiki/Code_refactoring) et sur [ses bienfaits](https://stackoverflow.com/questions/1727307/refactor-refactor-refactor-your-code-what-does-this-mean-exactly-and-why-do-i) puisque de nombreux sites s'en chargent déjà très bien. Je vais plutôt détailler un cas pratique.

<!--excerpt-->

## Raisons et buts

Pour bien comprendre, il faut savoir que je travaille sur une application Windows Mobile (ouch !), vieille de plus de 10 ans (argh !) et, bien entendu, à la **dette technique monstrueuse** : aucun test unitaire en 10 ans (avant que je n'implémente les premiers), duplication de code, code mort un peu partout, mauvais nommage des variables et fonctions, des fonctions obèses, etc. Cette application est destinée à être utilisée sur des PDAs et s'installe par carte SD. Il y avait donc, dans la solution de l'application, plusieurs projets, que je n'appellerai pas par leur vrai nom pour éviter tout problème éventuel.

*   Le projet principal, le **cœur** de l'application, les formes, le thread principal, [**Samus**](https://www.google.fr/search?q=samus+aran&client=firefox-b-ab&source=lnms&tbm=isch&sa=X&ved=0ahUKEwiPzJC4_anTAhVEthQKHSQ1AOwQ_AUICCgB&biw=1920&bih=971).
*   **L'installateur** de l'application sur le PDA, ainsi que le choix de la langue,[** Lana**](https://www.google.fr/search?q=lana+babenko&client=firefox-b-ab&source=lnms&tbm=isch&sa=X&ved=0ahUKEwjalezv9qnTAhWGWhQKHe7KAbYQ_AUIBigB&biw=1920&bih=971#tbm=isch&q=lana+beniko+swtor&imgrc=_).
*   Enfin, le projet servant à générer une partie de l'application, projet en pur C# desktop, répondant au doux nom de [**Lydia**](https://www.google.fr/search?q=lydia+skyrim&client=firefox-b-ab&source=lnms&tbm=isch&sa=X&ved=0ahUKEwiQytW7_anTAhUJVBQKHXUiDpoQ_AUIBigB&biw=1920&bih=971).

Sauf que Lydia ne fait pas vraiment son boulot. En fait, c'est en compilant Lana que l'on génère vraiment l'application, parce que Lana possède **un gros script post-build en Batch** (re-argh !), qui appelle d'autres batchs et qui finit par lancer Lydia, qui ne réalise que la génération d'un fichier nécessaire à l'installation. Le tout rend **Lana très lente**, mais ce n'est pas le pire. Non, le plus pénible, c'est que si Lana plante, elle se contentera, **comme message d'erreur, d'afficher tout le batch**, ce qui n'aide pas à deviner l'origine du problème (super, 200 lignes de batch en guise d'erreur). À cela s'ajoute **l'impossibilité de déboguer**, puisque il s'agit d'un script post-build et non d'un vrai projet (bon, où peut bien être la ligne qui foire dans ces 200 ?). Et comme c'est ce script qui lance Lydia, impossible de la déboguer elle aussi (héhé, débrouille-toi pour trouver quelle copie a tout fait planter). Enfin, de parce que 95% du process est composé de batch et que, dans les 5% restants, il est fait usage disproportionné de la réflexion (pour récupérer, par exemple, les langues utilisées des différents pays pour lesquels l'application est installable), le processus global de génération est **extrêmement lent**.

Il fallait donc remédier à ça en obtenant de Lydia qu'elle fasse son travail complet (et en C#, pas en batch), en obtenant de Lana qu'elle se génère sans aucun évènement post-build et en réorganisant tous les fichiers dispersés un peu partout entre Samus, Lana et Lydia.

## Le refactoring

On peut se demander par où commencer un tel travail. La première chose qui m'a paru nécessaire était de **réduire les dépendances** entre les projets autant que possible. Cela passe par **l'élimination de la réflexion** dans le code. Celle-ci était utilisée massivement par Lana pour proposer à l'utilisateur le choix des pays et de la langue, ainsi que par Lydia pour créer la carte en copiant les bonnes ressources et images. La réflexion n'apportait aucune valeur ajoutée pour contrebalancer le coût (d'écriture, de lecture et d'exécution), parce que les paramètres de langues et de pays ne bougent quasiment jamais. J'ai donc commencé par écrire un simple fichier *Package.cs* qui allait contenir les pays disponibles et les langues utilisables sur ces pays. Certes, il faudrait le modifier à la main pour ajouter un pays ou une langue, mais la rareté de l'opération le justifie.

{% highlight csharp %}
var packages = Country.GetAllCountries();

foreach (Package package in packages)
{
	var packageId = package.GetType().Name.Substring(8);

	if (Directory.Exists(@"StupidPath\\Install_" + packageId))
	{
		foreach (Type profileType in package.PackedProfiles)
		{
			var country = profileType.GetConstructor(new Type[] { }).Invoke(new object[] { }) as Country;
			// ...
		}
	}
}

/*
 *
 *
 */

public abstract class Country
{
	public static IEnumerable<Package> GetAllCountries()
	{
		List<Country> countries = new List<Country>();

		var types = Assembly.GetExecutingAssembly().GetTypes();
		foreach (var t in types)
		{
			if (typeof(Country).IsAssignableFrom(t) && !t.IsAbstract)
			{
				countries.Add(t.GetConstructor(new Type[] { }).Invoke(new object[] { }) as Country);
			}
		}

		return countries;
	}
}
{% endhighlight %}

Une fois ce fichier créé, j'ai pu modifier le code de Lana afin d'**éliminer les appels aux fichiers extérieurs non désirés** et **remplacer la réflexion par mon fichier statique**. Une fois ceci fait, il faut compiler et déployer afin de vérifier que Lana agit toujours de la même façon qu'avant. L'inconvénient de n'avoir aucun test unitaire, c'est qu'il faut tout tester à la main et qu'on a aucune garantie que tout marche à l'identique. Mais une fois que Lana est testée et qu'elle se révèle fiable, son code ne bougera plus et c'est une étape importante.

* * *

Maintenant, on passe à Lydia. Une majeure partie du refactoring se fait en **analysant le code existant**, en tentant de deviner l'utilité des paramètres (car oui, les variables sont très mal nommées) et en analysant plusieurs fois le processus de génération que l'on chercher à remplacer. Je finis par comprendre que Lydia fait deux choses : générer des fichiers .cab, qui semblent nécessaire à je ne sais quoi pour le PDA, et compiler les images en une archive .zip. Voilà déjà deux fonctionnalités que devra fournir ma nouvelle Lydia et je peux déjà écrire les prototypes des fonctions associées, que j'implémenterai plus tard.

Dans cette lancée, je retourne regarder les scripts post-build de Lana afin de déterminer clairement les autres étapes de génération et par là, les prototypes des fonctions équivalentes en C#. Il s'en dégage deux grands types.

*   Les **fonctions indépendantes** du pays ou de la langue et qui ne se lance qu'une fois. Dans ce genre, il y a la création de l’arborescence, ou bien le choix demandé à l'utilisateur, grande nouveauté de cette nouvelle version.
*   Les **fonctions liées** à un pays voire une langue, comme la compression et l'archivage des images ou la copie des fichiers dans les bons sous-répertoires.

Le développement commence dans le même ordre que la génération de la carte. Je commence notamment par la copie des images et des DLL, puis la création de l'arborescence, ensuite la génération des CAB et d'autres ZIP et enfin la copie sur la carte même. La description reste volontairement vague, mais l'idée essentielle est de **reprendre l'ordre de l'ancien code et de tester au fur et à mesure**. L'erreur à ne surtout : « C'est bon, je suis sur que ça marche, je vais faire plutôt cette fonction d'abord et ensuite tester le tout. » C'est vrai que c'est difficile quand certaines fonctions dépendent d'autres. Mais on peut s'en tirer, en bypassant la génération de ZIP par copie des ZIP tout faits dans le bon répertoire, par exemple. Un peu comme des tests unitaires. On mock, on encapsule, pour s'assurer qu'une fonction fait ce que l'on veut.

L'étape qui vient après et qui est, à mon goût, **la plus amusante et intéressante**, c'est **l'ajout de nouvelles fonctionnalités**. En effet, dès lors que l'on s'est assuré, dans la mesure du raisonnable, que le nouveau code fonctionne et produit les mêmes résultats que l'ancien, on peut passer à l'étape d'amélioration. J'ai ainsi pu ajouter un <span class="lang:c# decode:true crayon-inline ">ReadLine</span>  avec timeout pour le choix des pays, ce qui permet, lors de nos tests, de ne compiler que ceux qui nous intéressent et d'aller bien plus vite à générer la carte. D'ailleurs, si le code t'intéresse, je le mets en dessous. Il y a le lien StackOverflow dans les commentaires ; il est important de rendre les choses de César à César.

{% highlight csharp %}
/// <summary>
/// Delegate pour encapsuler ReadLine.
/// </summary>
private delegate string ReadLineDelegate();

/// <summary>
/// Encapsulation pour ajouter un timeout à Console.ReadLine.
/// </summary>
/// <param name="timeout">Timeout en ms.</param>
/// <returns>L'entrée de l'utilisateur ou string.Empty si timeout.</returns>
private static string ReadLine(int timeout)
{
	// http://stackoverflow.com/a/2041489/6060256

	ReadLineDelegate d = Console.ReadLine;

	IAsyncResult result = d.BeginInvoke(null, null);
	result.AsyncWaitHandle.WaitOne(timeout);

	if (result.IsCompleted)
	{
		// L'utilisateur a rentré quelque chose.
		string input = d.EndInvoke(result);
		return input;
	}
	else
	{
		// On a dépassé le temps max.
		return string.Empty;
	}
}
{% endhighlight %}

Une autre fonctionnalité que j'ai été content d'implémenter (grâce aux appsettings des projets C#), c'est de choisir si l'on veut compiler un pays dans son environnement de production ou de test. Comme il y a pas mal de fichiers qui changent, ça évite de devoir faire des copier-coller et des suppressions manuels.

## À venir

Je prévois d'autres améliorations, mais qui viendront quand j'aurais de nouveau du temps disponibles. L'un d'elle, qui nous fera gagner un temps fou, est de **compiler des bases de données embarquées**, utilisées dans des cas spéciaux, au moment de la création d'un pays. Jusqu'à maintenant, nous sommes obligés de le faire manuellement, ce qui est une énorme perte de temps. En fait, je ne fais qu'être un bon dev : s'il faut le faire deux fois à la main, c'est une de trop, il faut automatiser.

## Ce que ce refactoring m'a apporté

Déjà, de la **satisfaction**. C'est quelque chose d'agréable d'améliorer un produit, de rendre son utilisation plus simple ou plus agréable. J'adhère aux principes du software craftsmanship, donc je suis heureux de ces refactorings qui augmentent la qualité du code. Ensuite, j'ai acquis une **plus grande maîtrise** de l'application. Un processus de génération relativement obscur au début est bien plus clair dans ma tête. Il y a quelques petites subtilités que je n'ai pas encore saisies, mais dans sa globalité, je comprends mieux le projet. Enfin, le point le plus important, c'est **une plus grande expérience dans la gestion du code legacy** et du refactoring. Et ça, c'est important, parce que le code legacy n'est malheureusement pas quelque chose de peu commun, bien au contraire.

Le refactoring n'est pas encore achevé, mais vous avez eu un petit aperçu de ce que ça peut donner dans un cas plus pratique. Je sais que l'article manque de code, mais c'est une contrainte à laquelle je ne peux me soustraire vu que l'application est propriété de mon entreprise. Pour ceux qui veulent voir encore plus loin, je vous conseille [Working Effectively with Legacy Code](https://www.amazon.fr/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052).
