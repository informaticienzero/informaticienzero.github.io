---
layout: default
title: "Fichiers statiques partagés avec Django"
date: 2016-10-15 12:40:03
permalink: /:title/
---
Dans un projet web, il est fréquent de voir des fichiers CSS ou JavaScript partagés par tous les sous-projets. Django ne fait pas exception et nous permet de définir très simplement un dossier où l'on stockera tous nos statiques partagés. Il suffit de configurer `STATICFILES_DIRS` :

{% highlight python %}
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'))
{% endhighlight %}

Dans cet exemple, on indique à Django que le dossier **static** à la racine du projet doit aussi être analysé et utilisé pour les appels aux fichiers statiques.