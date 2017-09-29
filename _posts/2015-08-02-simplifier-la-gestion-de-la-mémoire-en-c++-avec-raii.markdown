---
layout: default
title: "Simplifier la gestion de la mémoire en C++ avec RAII"
date: 2015-08-02 20:53:03
permalink: /:title/
---
*Tutoriel publié à l'origine sur [Zeste de Savoir](https://zestedesavoir.com/tutoriels/460/simplifier-la-gestion-de-la-memoire-en-c-avec-raii/)*

La gestion des ressources est un problème récurrent en informatique. En effet, on ne dispose que de ressources limitées : RAM, disques durs, nombre de calculs par seconde, etc. Et aujourd'hui, il faut admettre qu'on charge de plus en plus de ressources qui prennent de la place. Il faut donc les gérer efficacement. Certains langages, comme le C, obligent l'utilisateur à allouer et libérer de la mémoire pour les ressources et il faut dire que c'est contraignant.

Le C++, en raison de l'approche historique qui en est malheureusement faite dans beaucoup d'ouvrages, est utilisé par certains développeurs comme le C, en gérant les ressources de manière manuelle. Pourtant, il existe un idiome très simple et efficace que nous allons découvrir dans ce tutoriel. Alors oubliez vos `new` et `delete` et découvrez ce que C++ offre.

<!--excerpt-->

*Un grand merci à [Davidbrcz](https://zestedesavoir.com/membres/voir/Davidbrcz/) pour son aide à l'amélioration de ce tutoriel, ainsi que tous ceux qui ont relevé des fautes (mention spéciale à [Dominus Carnufex](https://zestedesavoir.com/membres/voir/Dominus%2520Carnufex/)).*

## Gestion manuelle de la mémoire

Bien souvent, dès qu'on manipule des ressources externes, du type image à charger et afficher, connexion à une base de données, ou à un serveur, ou autres, il est inévitable de devoir réserver de la mémoire de façon dynamique. Pour ceux qui ont fait du C, vous pensez sans doute **aux pointeurs** et vous avez bien raison. Prenons donc un bête exemple : on se connecte à une base de données, on récupère un nombre fixé de noms de trains, on ouvre un fichier, on le verrouille, on travaille ensuite dessus avant de tout refermer comme il se doit.

{% highlight c %}
void get_infos_from_db()
{
	SGBD * sgbd = SGBD_Init("trains.db");
	const int nb_trains = 2;

	char** trains_name = malloc(nb_trains * sizeof(char*));
	for (int i = 0; i < nb_trains; ++i)
	{
		char buffer[256];

		trains_name[i] = malloc(42 * sizeof(char));
		sprintf(buffer, "SELECT name FROM trains WHERE id = %d", i);
		strcpy(trains_name[i], do_request(sgbd, buffer));
	}

	File * file = fopen("saved.txt");
	Lock * lock = lock_acquire();

	do_some_stuff(trains_name, file, lock);

	lock_release(lock);
	fclose(file), file = NULL;

	for (int i = 0; i < nb_trains; ++i)
	{
		free(trains_name[i]), trains_name[i] = NULL;
	}
	free(trains_name), trains_name = NULL;

	SGBD_release(sgbd);
}
{% endhighlight %}

Pourtant, ce code est juste une horreur à éviter. Pourquoi ? Parce qu'aucune vérification n'est faite. Si une seule opération échoue, on est bon pour un *segfault*. Alors, sécurisons ce code (merci à [Taurre ](https://zestedesavoir.com/membres/voir/Taurre/)).

{% highlight c %}
void darray_delete(void ** self, unsigned n)
{
	if (self != NULL)
	{
		unsigned i;
		for (i = 0; i < n; ++i) { free(self[i]); } free(self); } } void ** darray_create(unsigned n, unsigned m, size_t size) { void ** self; unsigned i; assert(n != 0 && m != 0 && size != 0); assert(SIZE_MAX / sizeof * self >= n);
	assert(SIZE_MAX / m >= size);

	self = malloc(n * sizeof * self);
	if (self == NULL)
	{
		goto alloc_array_fail;
	}

	for (i = 0; i < n; ++i)
	{
		self[i] = malloc(m * size);
		if (self[i] == NULL)
		{
			goto alloc_element_fail;
		}
	}

	return self;

alloc_element_fail:
	darray_delete(self, i);
alloc_array_fail:
	return NULL;
}

#define NTRAINS 2
#define TRAIN_MAX 42
#define BUFFER_MAX 256

void get_infos_from_db(void)
{
	SGBD * sgbd;
	void ** trains_name;
	FILE * file;
	Lock * lock;
	unsigned i;

	sgbd = SGBD_Init("trains.db");
	if (sgbd == NULL)
	{
		write_error_log(SGBD_FAIL);
		goto sgbd_fail;
	}

	trains_name = darray_create(NTRAINS, 1, TRAIN_MAX);
	if (trains_name == NULL)
	{
		write_error_log(TRAIN_CREATE_FAIL);
		goto darray_create_fail;
	}

	for (i = 0; i < NTRAINS; ++i)
	{
		char buffer[BUFFER_MAX];
		int n;

		n = spnrintf(buffer, sizeof buffer, "SELECT name FROM trains WHERE id = %d", i);
		if (n < 0 || n > sizeof buffer)
		{
			write_error_log(SNPRINTF_FAIL);
				goto snprintf_fail;
		}

		strcpy(trains_name[i], do_request(sgbd, buffer));
	}

	file = fopen("saved.txt");
	if (file == NULL)
	{
		write_error_log(FILE_FAIL);
		goto fopen_fail;
	}

	lock = lock_acquire();
	if (lock == NULL)
	{
		write_error_log(LOCK_FAIL);
		goto lock_acquire_fail;
	}

	/*
	 * Stuff
	 */

	lock_release(lock);

	lock_acquire_fail:
		fclose(file);

	fopen_fail:
	snprintf_fail:
		darray_delete(trains_name, NTRAINS);

	darray_create_fail:
		SGBD_release(sgbd);

	sgbd_fail:
		;
}

#undef NTRAINS
#undef TRAIN_MAX
#undef BUFFER_MAX
{% endhighlight %}

Quelle plaie à écrire ! Non seulement c'est long, mais en plus, c'est plus complexe à comprendre, on peut avoir oublié certains cas, bref, un cauchemar. Et encore, on aurait pu avoir à initialiser plus de ressources encore.

Peut-être certains d'entre vous pensent que `goto`, c'est un héritage du C dépassé, et qu'en C++ on devrait plutôt utiliser les exceptions. Soit, essayons.

{% highlight cpp %}
void get_infos_from_db()
{
	const int nb_trains = 2;
	SGBD * sgbd;

	try
	{
		sgbd = SGBD_Init("trains.db");
	}
	catch (sgbd_exception const & e)
	{
		write_error_log(e);
		throw;
	}

	char** trains_name;
	try
	{
		trains_name = new char*[nb_trains];
	}
	catch (std::bad_alloc const & e)
	{
		SGBD_release(sgbd);
		write_error_log(e);
		throw;
	}

	int last_good_alloc_index = 0;
	for (int i = 0; i < nb_trains; ++i)
	{
		char buffer[256];

		try
		{
			trains_name[i] = new char[42];
		}
		catch (std::bad_alloc const & e)
		{
			for (int i = 0; i < last_good_alloc_index; ++i)
			{
				delete[] trains_name[i], trains_name[i] = NULL;
			}

			delete[] trains_name, trains_name = NULL;
			SGBD_release(sgbd);
			write_error_log(e);
			throw;
		}

		last_good_alloc_index = i;

		sprintf(buffer, "SELECT name FROM trains WHERE id = %d", i);
		strcpy(trains_name[i], do_request(sgbd, buffer));
	}

	File * file;
	try
	{
		file = fopen("saved.txt");
	}
	catch (file_exception const & e) 
	{
		for (int i = 0; i < nb_trains; ++i)
		{
			delete[] trains_name[i], trains_name[i] = NULL;
		}

		delete[] trains_name, trains_name = NULL;
		SGBD_release(sgbd);
		write_error_log(e);
		throw;
	}

	Lock * lock;
	try
	{
		lock = lock_acquire();
	}
	catch (lock_exception const & e)
	{
		fclose(file), file = NULL;

		for (int i = 0; i < nb_trains; ++i)
		{
			delete[] trains_name[i], trains_name[i] = NULL;
		}

		delete[] trains_name, trains_name = NULL;
		SGBD_release(sgbd);
		write_error_log(e);
		throw;
	}

	do_some_stuff(trains_name, file, lock);

	lock_release(lock);
	fclose(file), file = NULL;

	for (int i = 0; i < nb_trains; ++i)
	{
		free(trains_name[i]), trains_name[i] = NULL;
	}
	free(trains_name), trains_name = NULL;

	SGBD_release(sgbd);
}
{% endhighlight %}

Finalement, ce code ne nous apporte aucun avantage par rapport au précédent : toujours aussi gros, toujours aussi illisible, et nous ne sommes même pas sûrs de couvrir tous les chemins possibles : un oubli est possible, une fonction apparemment inoffensive peut lancer une exception, bref, toujours un cauchemar à maintenir.

Que retenir jusque là : que la détection d'erreurs par retour de fonctions et `goto` ou par le biais d'exceptions nécessite d'ajouter des `if` ou des `try catch` toutes les deux lignes. En fait, dans ces cas de figure, chaque ligne où l'on acquiert une ressource qui n'est pas suivie d'un `if` ou entourée d'un `try catch` est suspecte et peut potentiellement faire échouer l'exécution.

Le cœur du problème tient en une phrase : le développeur doit écrire du code spécifique pour la libération de la mémoire et la gestion des erreurs. Pour améliorer la situation, il faut obligatoirement libérer le développeur de cette tâche, qu'elle soit automatique. Or, contrairement au C# ou au Java qui disposent d'un mécanisme de libération de la mémoire transparent et automatique appelé ***garbage collector***, il n'est rien de tel en C++[^garbage]. Sommes-nous donc condamnés à devoir écrire des codes aussi lourds ? Non, car une solution existe déjà.

## L'idiome RAII

Le C++ propose un idiome particulier appelé **RAII**, pour *Resource Acquisition Is Initialization*, ce que l'on peut traduire par « acquisition de ressources lors de l'initialisation » en français. Comment fonctionne-t-il ? Chaque ressource sera manipulée par une variable locale qui va l'acquérir à la construction et la libérer à la destruction. Ainsi, l'utilisateur n'aura même plus à se soucier d'appeler les fonctions `free`, `unlock` et autres `delete` pour que la libération des ressources ait bien lieu.

Pour appliquer cet idiome en C++, nous allons utiliser les classes et en particulier le couple constructeur(s) / destructeur. On peut parler de **capsules RAII**.

*   **Toutes les ressources seront acquises dans le constructeur** ; si des ressources sont impossibles à acquérir, on lève une exception. Ainsi, il n'y a pas de risque de créer un objet incomplet (*Ill formed* en anglais) donc pas de risque de fuite de mémoire : la norme garantit en effet que si un constructeur lève une exception, toute la mémoire des membres déjà allouée est libérée.
*   **Toutes les ressources seront libérées dans le destructeur**. Celui-ci étant appelé automatiquement dès que l'objet est détruit, on y écrira tous les mécanismes de libération de la ressource acquise dans le constructeur.


Un constructeur ne peut acquérir, au maximum, qu'une seule ressource non encapsulée par un mécanisme RAII. La classe contenant pour ce constructeur devient alors une capsule RAII pour cette ressource.


Voyons sans plus tarder comment appliquer ce principe à notre code précédent. Commençons par encapsuler nos ressources dans des classes, en prennant par exemple le SGBD.

{% highlight cpp %}
class Sgbd_Capsule
{
	public:
		Sgbd_Capsule(const char * db)
		{
			/*
				Appels de méthodes, initialisations d'attributs, etc.
			*/

			this->m_sgbd = SGBD_Init(db);
		}

		~Sgbd_Capsule()
		{
			/*
				On libère les ressources allouées.
			*/

			SGBD_release(this->m_sgbd);
		}

	private:
		SGBD * m_sgbd;
};
{% endhighlight %}

Maintenant, nous pouvons écrire du code aussi simple que celui ci-dessous (et nous verrons que nous pouvons faire encore plus simple dans la section suivante).

{% highlight cpp %}
int foo()
{
	Sgbd_Capsule sgbd("trains.db");

	/*
		Des opérations diverses sur le SGBD.
	*/

	return 42;
}
{% endhighlight %}

Les ressources sont libérées **à la sortie du bloc dans lequel nous les avons acquises**, c'est-à-dire ici en sortant de la fonction. Voyez par vous-mêmes l'exemple suivant.

{% highlight cpp %}
class Test
{
	public:
		Test(int number)
		: m_number(number)
		{
			std::cout << "Acquisition de la ressource n°" << this->m_number << std::endl;
		}

		~Test()
		{
			std::cout << "Libération de la ressource n°" << this->m_number << std::endl;
		}

	private:
		int m_number;
};

int main()
{
	Test a(1);
	{
		Test b(2);
		{
			Test c(3);
			Test d(4);
		}

		Test e(5);
	}
	return 0;
}
{% endhighlight %}

{% highlight shell %}
Acquisition de la ressource n°1
Acquisition de la ressource n°2
Acquisition de la ressource n°3
Acquisition de la ressource n°4
Libération de la ressource n°4
Libération de la ressource n°3
Acquisition de la ressource n°5
Libération de la ressource n°5
Libération de la ressource n°2
Libération de la ressource n°1
{% endhighlight %}

### Gestion des erreurs

Il reste néanmoins un problème que nous ne gérons pas encore : que fait-on si une erreur survient lors de l'acquisition ou de la libération des ressources ? Examinons chacun des cas.

#### Erreur lors de l'acquisition

Si on ne peut acquérir une ressource, alors l'objet ne peut être construit. Le mieux est donc de lancer une exception.

Lors de la construction d'un objet, si jamais le constructeur lance une exception, alors toute la mémoire réservée pour les membres sera libérée. Si jamais le constructeur a alloué de la mémoire dynamiquement de quelque manière que ce soit, alors cette dernière n'est pas libérée.

{% highlight cpp %}
class Sgbd_Capsule
{
	public:
		Sgbd_Capsule(const char * db)
		{
			/*
				Appels de méthodes, initialisations d'attributs, etc.
			*/

			this->m_sgbd = SGBD_Init(db);
			if (this->m_sgbd == nullptr)
			{
				throw sgbd_exception("Le SGBD ne peut être initialisé");
			}
		}

		~Sgbd_Capsule()
		{
			/*
				On libère les ressources allouées.
			*/

			SGBD_release(this->m_sgbd);
		}

	private:
		SGBD * m_sgbd;
};
{% endhighlight %}

Enfin, un conseil important que je répète : si on a plusieurs ressources à acquérir dans un même constructeur, il vaut mieux que chaque ressource soit encapsulée dans sa propre capsule RAII ; ainsi, chaque ressource sera libérée par son propre destructeur et on s'évite bien des soucis.

#### Erreur lors de la destruction

Ces cas-là sont problématiques. En effet, il est impossible de lancer une exception. Pourquoi ? Nous savons que le destructeur d'un objet sera appellé si une exception est lancée dans le code ; or, si le destructeur lance lui aussi une exception, nous nous retrouvons avec deux exceptions sur les bras, ce qui provoque un appel à la fonction `terminate()` et donc l'arrêt brutal du programme. De même, n'appelez jamais de fonctions dans le destructeur qui sont susceptibles de lancer des exceptions.

On peut néanmoins utiliser un système de logs pour informer l'utilisateur qu'une erreur dans la libération des ressources est arrivée. Quant à savoir si l'on continue l'exécution ou s'il vaut mieux tout arrêter, c'est à vous de voir en fonction des situations.

### Un mot sur le [dispose pattern](http://en.wikipedia.org/wiki/Dispose_pattern)

eut-être venez-vous d'un langage où il existe un mot-clef `finally`, utilisé à la suite d'un `try catch` et exécuté peu importe si une exception a été attrapée ou non ; ou bien existe-t-il des constructions similaires du type `using` (C#), `with` (Python) ou encore *`try`-with-ressources* (Java 7+). Dans tous les cas, le but est le même : empêcher des fuites de mémoire en libérant des ressources précédemment allouées. C'est ce qu'on appelle le [dispose pattern](http://en.wikipedia.org/wiki/Dispose_pattern).

Pourtant, C++ ne fournit pas de mot-clef ou de construction similaire à celles de Java ou C# pour la simple et bonne raison que RAII nous permet de faire la même chose de façon plus efficace. Qu'est-ce qui me permet de dire ça ? Je laisse le créateur du C++ [répondre](http://www.stroustrup.com/bs_faq2.html#finally).

** Bjarne Stroustrup **

> In a system, we need a "resource handle" class for each resource. However, we don't have to have an "finally" clause for each acquisition of a resource. In realistic systems, there are far more resource acquisitions than kinds of resources, so the "resource acquisition is initialization" technique leads to less code than use of a "finally" construct.

** Traduction libre **

> Dans un système, il faut une "capsule RAII" pour chaque ressource. Cependant, nous n'avons pas besoin d'une clause "finally" pour chaque acquisition de ressource. Dans des systèmes réalistes, il y a beaucoup plus d'acquisitions de ressources que de types de ressources, donc le RAII conduit à écrire moins de code que l'utilisation d'une construction avec "finally".

## Exemples d’application avec la bibliothèque standard

La bibliothèque standard utilise énormément cet idiome, à travers des noms qui vous sont certainement familliers : `std::string`, `std::array`, `std::vector`, `std::ifstream`, etc. Quand on y réfléchit, a-t-on déjà libéré manuellement un `std::string` ? Non, car c'est fait automatiquement pour nous. Et pour vous montrer à quel point la bibliothèque standard est infiniment supérieure à tout ce qu'on pourait faire manuellement, reprenons notre code de début en utilisant les mécanismes standards.

{% highlight cpp %}
void get_infos_from_db()
{
	const int nb_trains = 2;
	Sgbd_Capsule sgbd("trains.db");

	std::vector trains_names;
	for (int i = 0; i < nb_trains; ++i)
	{
		std::string buffer = "SELECT name FROM trains WHERE id = " + std::to_string(i);
		trains_names.push_back(do_request(sgbd, buffer));
	}

	std::ifstream file("saved.txt");
	Lock * lock = lock_acquire();

	do_some_stuff(trains_name, file, lock);

	lock_release(lock);
}
{% endhighlight %}

N'est-ce pas plus clair à lire et à comprendre ? Premier point à retenir : **toujours utiliser au maximum la bibliothèque standard**. Pourquoi se frustrer à faire un code comme on ferait en C quand on peut profiter de mécanismes éprouvés, performants et sûrs comme ceux proposés par la bibliothèque standard ? Donc faites-y appel le plus possible, ce sera du temps et du confort de gagnés.

### Cas particulier des pointeurs

Notre code n'est pas encore tout à fait satisfaisant. En effet, il reste un pointeur. Or, les pointeurs nus sont source de beaucoup de problèmes en C++. Et si on pouvait ne pas avoir à écrire `Sgbd_Capsule`, ce serait encore mieux. Heureusement, la bibliothèque standard arrive encore une fois à notre secours en fournissant des **pointeurs intelligents** qui nous libèrent des contraintes de libération que l'on connait si bien en C.

La norme C++11 nous propose plusieurs types de pointeurs intelligents :

*   `std::auto_ptr` : déprécié, à ne plus utiliser ;
*   `std::unique_ptr` : comme son nom l'indique, à utiliser quand on ne veut avoir qu'un seul pointeur sur un objet ;
*   `std::shared_ptr` : utilise un système de comptage de références qui permet que plusieurs pointeurs pointent un même objet, ce dernier étant libéré quand le dernier pointeur pointant dessus est détruit ;
*   `std::weak_ptr` : si l'on n'y prend pas garde, les `std::shared_ptr` peuvent entrainer un problème de références circulaires (lisez donc [cet article de Developpez](http://loic-joly.developpez.com/tutoriels/cpp/smart-pointers/#LIII-A-4) qui illustre ce problème). Il sert également dans le cas d'une ressource avec plusieurs observateurs non propriétaire. Je vous invite à lire [cet article](https://guillaumebelz.wordpress.com/2014/09/04/references-unique_ptr-shared_ptr-et-weak_ptr/) pour des explications plus approfondies sur lequel choisir.

Nous avons également deux *templates* bien pratiques :

*   `std::make_shared<T>` : construit un objet T et le met dans un `std::shared_ptr` (disponible avec C++11) ;
*   `std::make_unique<T>` : construit un objet T et le met dans un `std::unique_ptr` (disponible avec C++14, voir [ici](http://stackoverflow.com/questions/12580432/why-does-c11-have-make-shared-but-not-make-unique) pour une implémentation en C++11).

Ces *templates* sont à utiliser le plus possible car ils permettent d'écrire un code *exception-safe*. Lisez [l'article](http://herbsutter.com/2013/05/29/gotw-89-solution-smart-pointers/) de Herb Sutter à ce propos.

Et en plus, le mieux du mieux, on peut définir des *deleters*, c'est-à-dire définir comment le pointeur va libérer sa ressource. Il suffit simplement de créer une classe sur ce modèle que l'on passera ensuite en argument à notre pointeur intelligent.

{% highlight cpp %}
class Deleter
{
	public:
		template 
		void operator()(T * ptr) const
		{
			/* Opérations diverses pour libérer la ressource */
		}
};
{% endhighlight %}

Et comme un exemple vaut mille explications, utilisons ce principe avec notre SGBD et notre mécanisme de verrouillage qui se prêtent bien au jeu. Mais comme rien n'est parfait, les fonctions `std::make_shared<T>` et `std::make_unique<T>` ne prennent pas de *deleter* en argument. Il nous faut passer par la construction classique.

{% highlight cpp %}
class SGBD_deleter
{
	public:
		void operator()(SGBD * sgbd) const
		{
			SGBD_release(sgbd);
		}
};

class Lock_deleter
{
	public:
		void operator()(Lock * lock) const
		{
			lock_release(lock);
		}
};

void get_infos_from_db()
{
	const int nb_trains = 2;
	std::unique_ptr<SGBD, SGBD_deleter> sgdb {SGBD_Init("train.db"), SGBD_deleter()};

	std::vector trains_names;
	for (int i = 0; i < nb_trains; ++i)
	{
		std::string buffer = "SELECT name FROM trains WHERE id = " + std::to_string(i);
		trains_names.push_back(do_request(sgbd, buffer));
	}

	std::ifstream file("saved.txt");
	std::unique_ptr<Lock, Lock_deleter> lock {lock_acquire(), Lock_deleter()};

	do_some_stuff(trains_name, file, lock);
}
{% endhighlight %}

Les pointeurs intelligents nous permettent également d'éviter le problème du constructeur qui alloue lui-même de la mémoire que nous avons vu dans la section précédente. En effet, les pointeurs intelligents seront bien libérés même si l'on rencontre une exception. Donc utilisez-les dès que vous pouvez, quite à réécrire une version fonctionelle des pointeurs intelligents ou utiliser Boost si vous ne pouvez pas compiler en C++11 / C++14.

Deuxième point à retenir : chaque fois qu'il est nécessaire d'utiliser des pointeurs, **utilisez des pointeurs intelligents**. Les cas où vous devrez obligatoirement utiliser des pointeurs nus sont très rares, alors utilisez la solution la plus confortable.

## Bonnes pratiques

L'idéal, quand on gère des ressources, est de les **libérer dès que possible**. Non seulement cela est obligatoire dans certains cas (afin de ne pas faire attendre un processus trop longtemps pour ouvrir un fichier par exemple), mais en plus cela permet de soulager le système. Comment traduire cette bonne pratique en utilisant l’idiome RAII ? Eh bien, il faut que l’on détruise nos objets s’occupant des ressources le plus vite possible, ce qui est possible en utilisant des blocs d'instructions.

{% highlight cpp %}
int value;

{ // Début du bloc d’instructions
	std::ifstream f("test.txt");
	if (f.is_open()) {
		f >> value;
	}
} // Fin du bloc d’instruction : appel du destructeur du fichier

value = value * 4;
{% endhighlight %}

Il s’agit d’une pratique courante que vous pourrez voir dans certains codes. Et bien entendu, le corolaire : **ne déclarez vos objets que quand vous en avez besoin** et pas avant. Alors oubliez les réflexes du C89 qui consistent à déclarer toutes les variables au début d'un bloc et ne le faites que pour un usage immédiat (sauf exception).

### La const-correctness

Ce n'est pas une bonne pratique spécifique au RAII, mais dès qu'une ressource est censée être constante, alors il faut impérativement utiliser le mot-clef `const`. Cela donne des garanties à l'utilisateur et, couplé avec des références, permet un passage en argument plus rapide.

{% highlight cpp %}
void bar(std::string const & data)
{
	// Ici, on est certain que data ne sera pas modifiée.
	// On utilise le passage par référence pour éviter une recopie inutile.
}
{% endhighlight %}

D'ailleurs, petite astuce (merci [Herb Sutter](http://herbsutter.com/2013/04/05/complex-initialization-for-a-const-variable/)), si l'on veut déclarer un objet constant alors que ses paramètres dépendent de conditions, on peut y arriver grâce aux lambdas.

{% highlight cpp %}
#include <iostream>

class Test
{
	public:
		Test(int number)
		: m_number(number)
		{
		}

		int number() const
		{
			return this->m_number;
		}

	private:
		int m_number;
};

int main()
{
	const Test a(1);

	const Test f = [&]
	{
		Test f = Test(6);
		if (a.number() == 1)
		{
			f = Test(42);
		}

		return f;
	}();

	std::cout << "Valeur de f : " << f.number() << std::endl;

	return 0;
}
{% endhighlight %}

{% highlight shell %}
Valeur de f : 42
{% endhighlight %}

## Et dans les autres langages ?

Bien que le C++ ait été le précurseur et le plus grand utilisateur de l'idiome RAII, aujourd'hui, il n'est plus le seul. D'autres langages permettent, par des moyens assez similaires, d'utiliser une sorte de RAII.

### Avec C

Bien que cette possibilité soit offerte par une extension de GCC et donc non standard, elle mérite le détour et peut être intéressante pour ceux dont les applications ne seront compilées que par GCC. Il s'agit de l'attribut `cleanup`. Voici un exemple tiré de la [page](https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization) Wikipédia consacrée au RAII.

{% highlight c %}
static inline void fclosep(FILE ** fp)
{
	if (*fp)
		fclose(*fp);
}
#define _cleanup_fclose_ __attribute__((cleanup(fclosep)))

void example_usage()
{
	_cleanup_fclose_ FILE * logfile = fopen("logfile.txt", "w+");
	fputs("hello logfile !", logfile);

	/* logfile est correctement fermé sans appel explicite à fclose */
}
{% endhighlight %}

### Avec D

Le D fournit trois méthodes pour permettre la libération des ressources, dont une identique à celle utilisée en C++ : le couple constructeur / destructeur d'une classe. Les exemples suivants sont tirés du [site officiel](http://dlang.org/exception-safe.html).

{% highlight d %}
class Lock
{
	Mutex m;

	this(Mutex m)
	{
		this.m = m;
		lock(m);
	}

	~this()
	{
		unlock(m);
	}
}

void abc()
{
	Mutex m = new Mutex;
	auto l = scoped!Lock(new Lock(m));
	foo();
}
{% endhighlight %}

La seconde façon se rapproche de celle de Java avec un `try finally`.

{% highlight d %}
void abc()
{
	Mutex m = new Mutex;
	lock(m);        // lock the mutex
	try
	{
		foo();      // do processing
	}
	finally
	{
		unlock(m);  // unlock the mutex
	}
}
{% endhighlight %}

Enfin, il existe une troisième méthode, originale par rapport aux deux autres : `scope(exit)`. Tout le code qui sera placé après cette instruction sera exécuté peu importe si la fonction se termine normalement ou si une exception est lancée. Elle se décline également sous deux autres formes : `scope(failure)` où le code ne sera exécuté qu'en cas d'exception et `scope(success)` où le code sera exécuté en cas de déroulement normal. La [documentation](http://dlang.org/statement.html#ScopeGuardStatement) complètera mes explications.

{% highlight d %}
void abc()
{
	Mutex m = new Mutex;

	lock(m);                // lock the mutex
	scope(exit) unlock(m);  // unlock on leaving the scope

	foo();                  // do processing
}
{% endhighlight %}

### Avec Rust

Rust, langage développé par la fondation Mozilla, utilise le RAII de la même manière que C++. Et comme un code est plus parlant, voici celui tiré de [la page](http://rustbyexample.com/raii.html) consacrée au RAII avec Rust.

{% highlight rust %}
fn create_box() {
	// Allocate an integer in the heap
	let _function_box = box 3i;

	// `_function_box` gets destroyed here, memory gets freed
}

fn main() {
	// Allocate an integer in the heap
	let _boxed_int = box 5i;

	// new (smaller) scope
	{
		// Another heap allocated integer
		let _short_lived_box = box 4i;

		// `_short_lived_box` gets destroyed here, memory gets freed
	}

	// Create lots of boxes
	for _ in range(0u, 1_000) {
		create_box();
	}

	// `_boxed_int` gets destroyed here, memory gets freed
}
{% endhighlight %}

{% highlight shell %}
$ rustc raii.rs && valgrind ./raii
==26873== Memcheck, a memory error detector
==26873== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==26873== Using Valgrind-3.9.0 and LibVEX; rerun with -h for copyright info
==26873== Command: ./raii
==26873==
==26873==
==26873== HEAP SUMMARY:
==26873==     in use at exit: 0 bytes in 0 blocks
==26873==   total heap usage: 1,013 allocs, 1,013 frees, 8,696 bytes allocated
==26873==
==26873== All heap blocks were freed -- no leaks are possible
==26873==
==26873== For counts of detected and suppressed errors, rerun with: -v
==26873== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 2 from 2)
{% endhighlight %}
	
Nous voilà arrivés à la fin de ce tutoriel qui, je l'espère, vous en aura appris un peu plus sur C++. Bien entendu, le RAII n'est pas parfait : le pire qui puisse arriver est une erreur dans le destructeur. Mais hormis ces cas critiques, c'est un idiome particulièrement pratique et puissant, alors usez-en et abusez-en !

[^garbage]: Le C++ peut se voir doter d'un garbage collector. C'est quelque chose de prévu par la norme. Dans ce tutoriel, nous n'aborderons pas cette possibilité.
