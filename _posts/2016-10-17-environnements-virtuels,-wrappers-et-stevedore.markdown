---
layout: default
title: "Environnements virtuels, wrappers et stevedore"
date: 2016-10-17 22:44:47
permalink: /:title/
---
Quitte à reprendre un projet depuis zéro, ce qui m'arrive exactement aujourd'hui, autant repartir sur de bonnes bases. Le pythoniste en herbe que je suis devait donc apprendre à [utiliser les environnements virtuels](http://sametmax.com/les-environnement-virtuels-python-virtualenv-et-virtualenvwrapper/), un remède aux mots de tête face aux multiples versions du langages, des bibliothèques, des projets, etc.

Sauf que ça n'est pas marrant quand tout va bien, donc autant qu'il y ait des erreurs, ça pimente la soirée. Je suis la documentation et voilà ce que contient mon bashrc.

<!--excerpt-->

{% highlight shell %}
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/bin/virtualenvwrapper.sh
{% endhighlight %}

Et voilà ce que me crache à la figure ma console.

{% highlight shell %}
Traceback (most recent call last):
 File "/usr/lib/python3.5/runpy.py", line 184, in _run_module_as_main
 "__main__", mod_spec)
 File "/usr/lib/python3.5/runpy.py", line 85, in _run_code
 exec(code, run_globals)
 File "/usr/lib/python3.5/site-packages/virtualenvwrapper/hook_loader.py", line 16, in <module>
 from stevedore import ExtensionManager
ImportError: No module named 'stevedore'
virtualenvwrapper.sh: There was a problem running the initialization hooks. 

If Python could not import the module virtualenvwrapper.hook_loader,
check that virtualenvwrapper has been installed for
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3 and that PATH is
set properly.
{% endhighlight %}

Arf, pas cool ce [chef des docks](https://fr.wikipedia.org/wiki/Stevedore). Mais à force de chercher et grâce à ce merveilleux site qu'est StackOverflow, j'ai pu trouver [la réponse](https://stackoverflow.com/questions/32086631/cant-install-virtualenvwrapper-on-osx-10-11-el-capitan/32095725#32095725). Allez, on la remets ici pour la postérité.

{% highlight shell %}
sudo pip install pbr
sudo pip install --no-deps stevedore
sudo pip install --no-deps virtualenvwrapper
{% endhighlight %}$

Ça marche ! C'est pas un ordinateur qui va me résister, rogntudju !

[![](http://lagaffemegate.free.fr/agace/845.jpg)](http://lagaffemegate.free.fr/agace/845.jpg)