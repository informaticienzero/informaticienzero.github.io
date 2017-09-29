---
layout: default
title: "Lambdas, objet constant, C++"
date: 2017-04-07 15:59:34
permalink: /:title/
---
C'est une astuce que j'avais trouvé il y a quelques temps déjà en écrivant un tutoriel sur RAII en C++, mais que j'ai envie de partager avec vous parce que c'est simple mais astucieux. Grâce à un lambda, on peut initialiser une variable déclarée constante, alors que sa valeur dépend d'une condition.

<!--excerpt-->

{% highlight c %}
#include <iostream>

struct Test
{
	Test(int number)
		: number(number)
	{
	}

	int number;
};

int main()
{
	const Test a(1);

	const Test f = [&]
	{
		Test f(6);
		if (a.number == 1)
		{
			f = Test(42);
		}

		return f;
	}();

	std::cout << "Valeur de f : " << f.number << std::endl;

	// Erreur, f est constant !
	//f.number = 5;

	return 0;
}
{% endhighlight %}

L'exemple est bateau, mais illustre bien la puissance des lambdas. Heureusement que cette fonctionnalité puissante est standard en C++ depuis 6 ans !