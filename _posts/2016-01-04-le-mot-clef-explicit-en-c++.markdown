---
layout: default
title: "Le mot-clef explicit en C++"
date: 2016-01-04 20:21:21
permalink: /:title/
---
J'ai eu l'occasion d'aider quelqu'un récemment sur [Zeste de Savoir](https://zestedesavoir.com/forums/sujet/4981/explicit/). Cette personne demandait des explications sur le mot-clef du titre. Je pense donc qu'un petit article dessus peut servir. Ce mot-clef sert dans deux cas.

*   Le(s) constructeur(s) d'une classe.
*   La surcharge de l'opérateur de conversion.

<!--excerpt-->

Il s'agit dans tous les cas de dire au compilateur de **ne PAS faire de conversion implicite** lors de l'appel d'une fonction. Voici un exemple typique tiré (et légèrement adapté) de [StackOverflow](https://stackoverflow.com/questions/121162/what-does-the-explicit-keyword-mean).

{% highlight cpp %}
class Foo
{
	public:
	Foo (int foo) : m_foo (foo) 
	{
	}

	int get_foo () { return m_foo; }

	private:
		int m_foo;
};

void do_bar(Foo foo)
{
	int i = foo.get_foo();
}

int main ()
{
	do_bar(42);
}
{% endhighlight %}

Ici, le compilateur va implicitement convertir notre **int** en **Foo(int)** et tout va bien se passer. Dans le cas où l'on rajoute **explicit** devant le constructeur, ça ne marche plus et l'on est obligé d'écrire le code suivant.

{% highlight cpp %}
int main ()
{
	do_bar(Foo(42));
}
{% endhighlight %}

Le principe est sensiblement le même avec l'opérateur de conversion. Et comme un exemple (voire plusieurs) est plus parlant que des mots.

{% highlight cpp %}
struct test {
  operator bool() { return true; }
};

int main() {
  test t;
  if(t) { ... }
}

// Sans explicit.
struct S { S(int) { } /* ... */ };

	struct SS {
		int m;
		SS(int x) :m(x) { }
		operator S() { return S(m); }  // because S don't have S(SS); non-intrusive
	};

	SS ss(1);
	S s1 = ss;  // ok; like an implicit constructor
	S s2(ss);   // ok ; like an implicit constructor
	void f(S);
	f(ss);      // ok; like an implicit constructor

// Avec explicit.
struct S { S(int) { } };

	struct SS {
		int m;
		SS(int x) :m(x) { }
		explicit operator S() { return S(m); }  // because S don't have S(SS)
	};

	SS ss(1);
	S s1 = ss;  // error; like an explicit constructor
	S s2(ss);   // ok ; like an explicit constructor
	void f(S); 
	f(ss);      // error; like an explicit constructor
{% endhighlight %}

Ces exemples sont tirés respectivement de [StackOverflow](https://stackoverflow.com/questions/1307876/how-do-conversion-operators-work-in-c/1308335#1308335) et du [site de Bjarne Stroustrup](http://www.stroustrup.com/C++11FAQ.html#explicit-convertion). Et si tu regarde la bibliothèque standard, tu verras que ce mot-clef est souvent utilisé. Il permet de se prémunir des pièges que peuvent poser les conversions implicites, héritées du C.
