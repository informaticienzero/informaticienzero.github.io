---
layout: default
title: "Commandes personnalisées pour manage.py"
date: 2016-11-07 07:45:31
permalink: /:title/
---
Tout utilisateur de Django connait bien la commande `python manage.py`, qu'on lance pour collecter les fichiers statiques, appliquer des migrations ou créer un super utilisateur. Tiens ! en parlant de ça, j'étais face à un problème : je devais créer un super utilisateur mais **le projet se trouve à l'intérieur d'un conteneur**, donc **pas d'accès au shell**, donc pas possible de rentrer les informations.

<!--excerpt-->

Heureusement, Django est un projet bien conçu et il est possible de **créer nous-mêmes nos propres commandes**. Il faut simplement respecter certaines procédures (merci à [ce blog](http://janetriley.net/2014/11/quick-how-to-custom-django-management-commands.html)).

*   Créer un package (donc avec un `__init__.py`) d'un certain nom (disons **shell**) et l'ajouter aux `INSTALLED_APPS` du projet.
*   Dans ce premier package, créer un deuxième package appelé `management`.
*   Dans ce deuxième package, créer un troisième du nom de `commands`.
*   Dans ce troisième package, créer autant de fichiers que l'on veut, donc les noms deviendront des commandes utilisables avec `python manage.py`.

Voilà un *tree* pour bien comprendre, avec la commande pour créer le super utilisateur sans input.

{% highlight shell %}
.
    ├── shell
    │   ├── __init__.py
    │   ├── management
    │   │   ├── commands
    │   │   │   ├── createsu.py
    │   │   │   ├── __init__.py
    │   │   ├── __init__.py
{% endhighlight %}

Et voici la commande qui me permet de créer mon super utilisateur et qui se lance aussi simplement que ça : `python manage.py createsu`

{% highlight python %}
from django.contrib.auth import get_user_model
from django.core.management.base import BaseCommand
import os

# The class must be named Command, and be subclass of BaseCommand.
class Command(BaseCommand):
	"""
		This command allows to create a superuser without any prompt.
		It is usefull inside a Docker container because we have no shell available.
	"""

	# A command must define handle().
	def handle(self, *args, **options):

		name = 'informaticienzero'
		mail = 'informaticienzero@gmail.com'
		password = 'informaticienzero'

		# If we're in prod, overwrite the parameters.
		if os.environ['DJANGO_PRODUCTION'] == 'true':
			name = os.environ['SU_NAME']
			mail = os.environ['SU_MAIL']
			password = os.environ['SU_PASSWORD']

		user = get_user_model()
		if not user.objects.filter(username=name).exists():
			user.objects.create_superuser(name, mail, password)
{% endhighlight %}