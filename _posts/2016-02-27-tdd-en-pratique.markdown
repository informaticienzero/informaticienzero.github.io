---
layout: default
title: "TDD en pratique"
date: 2016-02-27 22:57:33
permalink: /:title/
---
Tout le monde connait le site des pages blanches. Avec ce service, on peut connaître les habitants d'une rue ou bien retrouver l'adresse d'une personne à partir de son nom. Je me suis donc dit que j'allais faire un programme permettant de connaître tous les habitants d'une ville, simplement en tapant son nom. Tant qu'à faire un projet d'entraînement et à expérimenter, j'ai décidé de tester le **Test Driven Development.**

<!--excerpt-->

Mon code est librement disponible [sur mon Github](https://github.com/informaticienzero/Pages-blanches). S'il y a des personnes souhaitant réutiliser le code, pas de soucis, il est sur GH pour ça !

# Test Driven Development

Traduit en français par **développement piloté par les tests**, TDD consiste à écrire les tests avant d'écrire les méthodes testées. Plus précisément, il y a un découpage en 5 étapes.

*   L'ajout d'une fonctionnalité ne doit se faire (en théorie) qu'après avoir bien compris les spécifications et le but de celle-ci. Dans un contexte agile, cela peut se faire par des uses cases et des uses stories. En fait, ce qui est surtout important, c'est que le développeur **se concentre vraiment sur ces spécifications et le fonctionnement de la fonctionnalité plus que sur le code**. On connait tous cette fâcheuse tendance à ne se concentrer que sur le code, en perdant de vue pourquoi on l'écrit. On commence donc par coder d'abord le test avant toute chose.
*   La deuxième étape est un principe de sûreté qui consiste à lancer le test, alors même que la ou les fonction(s) testée(s) ne sont pas implémentées. Le but est de **garantir que le test n'est pas validé par erreur** et qu'il n'échoue pas à cause d'une erreur autre que le fait que l'implémentation ne corresponde pas à la fonctionnalité.
*   Maintenant seulement on peut commencer à **écrire cette implémentation**, mais de façon **minimale**, juste de quoi faire passer le test, ni plus ni moins.
*   On lance le test, puis on ajuste l'implémentation si le test ne passe pas. Cette façon de faire permet de s'assurer que l'implémentation est correcte et qu'elle n'entraîne pas de régression par rapport à l'existant.
*   On recommence le processus à chaque ajout ou modification de fonctionnalité. L'autre principe est de faire un nettoyage du code en supprimant la duplication, en renommant ce qui était mal nommé, en redécoupant des classes, des fichiers ou des fonctions, bref, **faire du refactoring**, afin de garder le code le plus propre possible. Ces deux aspects sont préconisés par l'Extreme Programming, mais là n'est pas le sujet d'aujourd'hui.

[caption id="" align="aligncenter" width="1421"]![](https://upload.wikimedia.org/wikipedia/commons/0/0e/Cycle-global-tdd.png) TDD tiré de [la page Wikipédia](https://fr.wikipedia.org/wiki/Test_driven_development) consacrée au sujet (téléversé par Xarawn sous licence CC BY-SA 4.0).[/caption]

Si cette pratique est de plus en plus conseillée (il suffit de faire une recherche dans son moteur favori pour s'en rendre compte), c'est qu'elle apporte un certain nombre de bénéfices.

*   Une **documentation vivante et pratique** du code est mise en place. En effet, les tests doivent valider que le code répond aux spécifications, donc ils collent au plus près de celles-ci. De plus, ils permettent, d'un rapide examen, de comprendre comment fonctionne le code (les arguments attendus, les retours de fonction, etc).
*   Des **garanties contre la régression**. Si chaque fonctionnalité est codée de cette façon, il suffit, lors d'une modification, de relancer les tests et d'observer si certains échouent pour prouver qu'une régression a été introduite.
*   Des **garanties de qualité**. Les tests sont automatisables et peuvent être lancés à chaque build, à chaque commit, ou autre. On a ainsi un rapport immédiat et clair de la qualité du code. Couplés à un outil d'analyse de qualité comme SonarQube, on peut ainsi déceler quelles portions méritent plus de tests et augmenter encore la qualité globale. De plus, comme les tests sont faits avant le code, on évite le syndrome du test écrit pour valider le code et donc biaisé.
*   Une **base de tests**. Parce que oui, on est des gros flemmards et des fois, on n'écrit pas les tests ou ceux-ci ne sont pas assez complets. En pratiquant le TDD, on garantit la présence la présence d'une base de tests solides.

Bon, comme partout en informatique, rien n'est parfait. Ainsi, même si TDD aide à la qualité du code, il ne garantit en rien de passer les tests d'intégration ou de recette. De même, il ne permet pas de se protéger à 100% des régressions ou des erreurs. Et tous les codeurs n'ont pas le même niveau pour mettre en places ces tests. Et surtout, si TDD est facile à mettre en place sur un projet neuf, ce n'est pas la même paire de manches (ou devrais-je dire de gants ?) sur une base de code existante, surtout quand celle-ci n'est pas la plus propre qui soit.

# Ce que ça donne

Bon je parle, je parle et je sens que certains commencent à dormir au fond de la classe (oui, je t'ai vu toi là-bas). Alors parlons peu, parlons concret. Qu'est-ce que TDD a donné appliqué à mon projet ?

Prenons le cas de la classe ***TestCSVAddressParser***. Mon but était de faire un parseur qui analyserait un fichier CSV contenant un grand nombres d'adresses, tirées des [bases de données](https://www.data.gouv.fr/fr/datasets/base-d-adresses-nationale-ouverte-bano/) librement accessibles du gouvernement. Je me suis dit qu'il me faudrait être capable de tirer des informations d'une seule ligne, d'un fichier entier et d'être capable d'éliminer la redondance d'adresses. Partant de ça, j'ai commencé les tests pour une ligne valide, pour un ligne non-valide ainsi que pour une ligne vide. Puis, m'étant assuré que ces fonctions étaient correctes et répondaient à mes besoins, j'ai pu passer à l'analyser de plusieurs lignes.

L'avantage ? Non seulement, quand mes fonctions pour plusieurs lignes ne marchaient pas, j'ai pu accélérer la correction puisque j'avais la garantie de la lecture d'une ligne était fonctionnelle, mais en plus, rien qu'en regardant le fichier de test, on comprend rapidement le fonctionnement du code de ***CSVAddressParser***.

{% highlight csharp %}
using Microsoft.VisualStudio.TestTools.UnitTesting;
using PagesBlanches;
using System.Collections.Generic;
using System.Linq;

namespace UnitTestsPagesBlanches
{
	[TestClass]
	public class TestCSVAddressParser
	{
		/// 
		/// Une ligne valide d'un CSV valide.
		///
		private static string ValidLine = @"ADRNIVX_0000000350549458;rue evariste galois;0422;27;"";95563;95320;"";"";RUE EVARISTE GALOIS;ST LEU LA FORET;645099.9;6878890.2;2.24958878684424;49.0075769064767;Saint-Leu-la-Forêt";

		///
		/// Une ligne valide mais qui ne contient aucune information.
		///
		private static string ValidEmptyLine = @";;;;;;;;;;;;;;;";

		///
		/// Teste le cas d'une ligne valide avec les bonnes informations.
		///
		[TestMethod]
		public void ParseValidLine()
		{
			// Arrange.
			var informations = CSVAddressParser.ParseSingleLine(TestCSVAddressParser.ValidLine);

			// Act.
			int inseeCode;
			bool isInseeCodeValid = int.TryParse(informations[CSVAddressParser.Informations.INSEE_CODE], out inseeCode);

			int zipCode;
			bool isZipCodeValid = int.TryParse(informations[CSVAddressParser.Informations.ZIP_CODE], out zipCode);

			string cityName = informations[CSVAddressParser.Informations.CITY_NAME];
			string streetName = informations[CSVAddressParser.Informations.STREET_NAME];

			// Assert.
			Assert.AreEqual(CSVAddressParser.InformationsPerLine, informations.Count);
			Assert.IsTrue(isInseeCodeValid);
			Assert.IsTrue(isZipCodeValid);
			Assert.IsNotNull(cityName);
			Assert.IsNotNull(streetName);
			Assert.AreNotEqual(cityName.Length, 0);
			Assert.AreNotEqual(streetName.Length, 0);
		}

		///
		/// Teste le cas où une ligne n'a pas d'information, juste le bon nombre de point-virgules.
		///
		[TestMethod]
		public void ParseValidEmptyLine()
		{
			var informations = CSVAddressParser.ParseSingleLine(TestCSVAddressParser.ValidEmptyLine);
			foreach (var information in informations)
			{
				Assert.IsNotNull(information.Value);
				Assert.IsTrue(string.IsNullOrEmpty(information.Value));
			}
		}

		///
		/// Teste un fichier sans redondance, où chaque rue est unique.
		///
		[TestMethod]
		public void ParseValidFileWithoutMultiples()
		{
			// Arrange.
			var informations = CSVAddressParser.ParseMultipleLines(Properties.Resources.ValidDifferentAddresses);
			var cities = new List();
			var addresses = new List();

			// Act.
			foreach (var information in informations)
			{
				cities.Add(information[CSVAddressParser.Informations.CITY_NAME]);
				addresses.Add(CSVAddressParser.StreetName(information));
			}

			// Assert.
			Assert.AreEqual(cities.Count, informations.Count);
			Assert.AreEqual(addresses.Count, informations.Count);
			for (int i = 0; i < informations.Count; ++i)
			{
				Assert.IsFalse(string.IsNullOrEmpty(cities[i]));
				Assert.IsFalse(string.IsNullOrEmpty(addresses[i]));
			}
		}

		///
		/// Teste un fichier avec des redondances.
		///
		[TestMethod]
		public void ParseValidFileWithMultiples()
		{
			// Arrange.
			var informations = CSVAddressParser.ParseMultipleLines(Properties.Resources.ValidMultiplesAddresses);
			var cities = new List(); var addresses = new List();

			// Act.
			foreach (var information in informations)
			{
				cities.Add(information[CSVAddressParser.Informations.CITY_NAME].Replace("\\r", string.Empty));
				addresses.Add(CSVAddressParser.StreetName(information).Replace("\\r", string.Empty));
			}

			cities = cities.Distinct().ToList();

			// Assert.
			Assert.AreEqual(informations.Count, 3);
			Assert.AreEqual(cities.Count, 2);
			Assert.AreEqual(addresses.Count, 3);
		}
	}
}
{% endhighlight %}

Bon, qu'on se le dise, **ça n'a pas été facile de se débarrasser des vieux réflexes** qui consistent à tester après avoir codé. Des fois, j'ai ajouté trop de fonctionnalités d'un coup sans les tester au même moment, ce qui est l'opposé de ce que préconise TDD. En plus, qu'on se le dise, c'est plus lent que de rentrer directement dans le lard, sans prendre de gants. Mais j'ai beaucoup aimé cette façon de faire, différente de ce que j'avais vu jusqu'ici (surtout dans le cadre professionnel où la base de code est énorme et où TDD n'est pas du tout mis en pratique). Le projet est trop petit et sans enjeu, donc difficile de dire si TDD a tenu ses promesses dans mon cas. Mais je réessayerai.

Dans le prochain article, je parlerai de quelque chose d'autres que j'ai mis en pratique avec ce projet : **la programmation par contrat**.
