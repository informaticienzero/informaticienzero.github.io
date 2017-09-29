---
layout: default
title: "C++ et les « functions try blocks »"
date: 2017-01-06 16:05:37
permalink: /:title/
---
C++ est vraiment un langage qui ne cesse jamais de me surprendre, tant par sa complexité que sa puissance. Et bien que je trouve qu'il manque de clarté par rapport à des langages plus récents (comme C#), il n'empêche qu'**il m'émerveille**. La découverte du jour, ce sont les « *functions try blocks* ». C'est, en très simple, la possibilité de faire un `try catch` quand on utilise la liste d'initialisation.

<!--excerpt-->

Cette liste d'initialisation trouve son intérêt dans plusieurs cas, notamment l'initialisation de constantes, de références ou l'appel au(x) constructeur(x) d'éventuelle(s) classe(s) mère(s), auxquels cas elle est obligatoire.

{% highlight cpp %}
#include <iostream>

struct Mother
{
	Mother(int value)
		: x(value)
	{
		std::cout << "Calling Mother::Mother(int).\\n";
	}

	private:
		int x;
};

struct Daughter : public Mother
{
	const int random_value;

	Daughter()
		: Mother(42), random_value(77)
	{
		std::cout << "Calling Daughter::Daughter().\\n";
		std::cout << "We have a constant random value of " << this->random_value << std::endl;
	}
};

int main()
{
	Daughter d;
	return 0;
}
{% endhighlight %}

Mais que faire si quelque chose claque une exception dans ma liste d'initialisation ? Le constructeur de ma classe mère peut très bien me péter dans les pattes sans que ça ne lui pose un quelconque problème de conscience. Et en utilisant la liste d'initialisation, je ne peux pas enrober le tout d'un `try catch` puisqu'il serait en dehors d'une fonction. Voilà le moment où les  *functions try blocks* entrent en scène. Dans notre cas, ça donne ceci.

{% highlight cpp %}
struct Mother
{
	Mother(int value)
		: x(value)
	{
		std::cout << "Calling Mother::Mother(int).\\n";
		throw 42;
	}

	private:
		int x;
};

struct Daughter : public Mother
{
	Daughter()
		try : Mother(42)
	{
		std::cout << "Calling Daughter::Daughter().\\n";
	}
	catch (int ex)
	{
		std::cout << "Oops, got a silly exception.\\n";
	}
};
{% endhighlight %}

Et là, en tant que lecteur curieux que tu es, tu n'as pu t'empêcher de tester ce code et tu t'es rendu compte d'une horrible vérité : **le programme s'arrête brutalement dû à une exception non rattrapée**. Comment est-ce possible ? Notre catch serait-il un traître ? Tout ce tralala serait-il purement et simplement inutile ?

Pour comprendre cette absence de rattrapage, il faut répondre à la question : un objet dont le constructeur lance une exception peut-il est considéré comme valable et utilisable ? Si une exception est lancée, c'est que quelque chose ne va pas et que l'objet ne devrait donc pas être créé. Si on outre-passe ceci, on se retrouve avec un objet « bâtard », mal formé, qu'on appelle *ill-formed* en anglais.

En effet, comme prévenir l'utilisateur que son objet n'est pas pleinement formé, autrement qu'avec une exception ? Une sorte de fonction `is_ok` ? C'est l'assurance qu'un jour, quelqu'un oubliera de l'appeler et que tout ce joli bazar lui explosera à la tronche sans la moindre semonce.

Le seul et unique but de ce mécanisme est de **permettre une traduction de l'exception avant de la relancer**, **ou bien de logger**. Rien d'autre. L'exception doit remonter et être rendue visible à l'utilisateur.

{% highlight cpp %}
#include <iostream>

struct Mother
{
	Mother(int value)
		: x(value)
	{
		std::cout << "Calling Mother::Mother(int).\\n";
		throw 42;
	}

	private:
		int x;
};

struct Daughter : public Mother
{
	Daughter()
		try : Mother(42)
	{
		std::cout << "Calling Daughter::Daughter().\\n";
	}
	catch (int ex)
	{
		std::cout << "Oops, got a silly exception.\\n";
	}
};

int main()
{    
	try
	{
		Daughter d;
	}
	catch (int ex)
	{
		std::cout << "In main. Now I do what I want (retry, abort, launching a nuke, etc).\\n";
	}

	return 0;
}
{% endhighlight %}

Pour aller plus loin, je t'invite à lire [cet article de Dr.Dobb's](http://www.drdobbs.com/introduction-to-function-try-blocks/184401297) ainsi que [le GotW #66](http://www.gotw.ca/gotw/066.htm) de Herb Sutter.
