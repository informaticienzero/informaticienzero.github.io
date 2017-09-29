---
layout: default
title: "Les concepts avec C++17"
date: 2017-01-12 16:10:36
permalink: /:title/
---
Enfin ça ne sera pas disponible avec le standard de 2017 (malheureusement !), mais on peut déjà faire mu-muse avec, notamment avec GCC 7 et Clang 4, que l'on peut retrouver sur [Wandbox](http://melpon.org/wandbox). Il faudra simplement compiler avec l'option `-fconcepts`.

<!--excerpt-->

## Les joies des templates

Pour bien comprendre pourquoi les concepts sont si intéressants en C++, il suffit de s'être mangé une erreur de templates dans les dents au moins une fois. Prenons [cet exemple](https://codegolf.stackexchange.com/questions/1956/generate-the-longest-error-message-in-c/10470#10470) tiré de Stack Overflow.

{% highlight cpp %}
#include <vector>
#include <algorithm>

int main()
{
	int a;
	std::vector<std::vector<int>> v;
	auto it = std::find(v.begin(), v.end(), a);
}
{% endhighlight %}

L'erreur, postée sur SO, vient du simple fait que l'objet `a` n'est pas du type attendu (`std::vector<int>`). Et les messages à rallonges n'aident pas à comprendre, tant chez GCC que chez Clang.

## Les concepts : une TS pleine d'espoir

Évidemment, il se peut que l'implémentation, la syntaxe, etc changent, ou bien que de nouvelles choses soient ajoutées, puisque ce n'est qu'une spécification technique et absolument pas quelque chose de définitif. Mais on peut faire déjà des choses intéressantes, comme le code ci-dessous.

{% highlight cpp %}
#include <algorithm>
#include <iostream>
#include <type_traits>

template<typename T>
concept bool Integral() 
{
	return std::is_integral<T>::value;
}

template<typename T>
concept bool StructWithInteger = requires(T param)
{
	param.i;
};

template<typename T>
concept bool Structure()
{
	return StructWithInteger<T> && Integral<decltype(T::i)>();
}

template <Structure T>
void f(T param)
{
	std::cout << "T::i == " << param.i << std::endl;
}

struct A
{
	int i = 42;
};

struct B
{
	int * i = nullptr;
};

struct C
{
	double i = 2.71818;
};

int main()
{
	f(A{});
	//f(B{}); // Isn't working and perfectly normal, because (int*) is not (int).
	//f(C{}); // Isn't working and perfectly normal, because (double) isn't (int).

	return 0;
}
{% endhighlight %}

Dans ce code, je décide que je ne veux en argument qu'une structure contenant un entier `i`. Si je décide de passer un pointeur, par exemple, voilà ce que j'obtiens comme erreur.

{% highlight cpp %}
prog.cc: In function 'int main()':
prog.cc:47:17: error: cannot call function 'void f(T) [with T = B]'
	 f(B{}); // Isn't working and perfectly normal, because (int*) is not (int).
				 ^
prog.cc:24:6: note:   constraints not satisfied
 void f(T param)
	  ^~~~~~~~
prog.cc:18:14: note: within 'template<class T> concept bool Structure() [with T = B]'
 concept bool Structure()
			  ^~~~~~~~~
prog.cc:6:14: note: within 'template<class T> concept bool Integral() [with T = int*]'
 concept bool Integral()
			  ^~~~~~~~
prog.cc:6:14: note: 'std::is_integral<int*>::value' evaluated to false
{% endhighlight %}

Eh oui ! un pointeur n'est pas un entier, tout simplement. Et si je passe un entier mais qui n'a pas le bon identifiant ?

{% highlight cpp %}
prog.cc: In function 'int main()':
prog.cc:46:17: error: cannot call function 'void f(T) [with T = A]'
	 f(A{});
				 ^
prog.cc:24:6: note:   constraints not satisfied
 void f(T param)
	  ^~~~~~~~
prog.cc:24:6: note: in the expansion of concept '(Structure<T>)()' template<class T> concept bool Structure() [with T = A]
prog.cc:20:36: error: 'i' is not a member of 'A'
	 return StructWithInteger<T> && Integral<decltype(T::i)>();
									^~~~~~~~~~~~~~~~~~~~~~~~
{% endhighlight %}

C'est donc vraiment quelque chose de puissant qui se dessine. On peut ainsi rêver d'avoir la puissances des templates C++ avec les contraintes comme en C#, pour un codage et des messages d'erreurs plus simples. Voici, en guise de conclusion, des liens vers tout un tas de ressources qui m'ont aidé à appréhender les concepts.

- [Le blog C++ d'](https://akrzemi1.wordpress.com/2012/01/11/concept-axioms-what-for/)[Andrzej].
- Une [collection d'exemples](https://github.com/asutton/origin) sur Github.
- [Le blog de Tom Honermann](http://honermann.net/blog/category/c-concepts/).
- Un [brouillon](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4377.pdf) de l'ISO sur les concepts.
- Un [article](http://ericniebler.com/2013/11/23/concept-checking-in-c11/) expliquant comment s'en tirer en C++11, avec notamment les traits et les `static_assert`.
- L'incontournable Boost, qui propose déjà [une solution](http://www.boost.org/doc/libs/1_61_0/libs/concept_check/concept_check.htm).