---
layout: default
title: "Makefile, espaces, tabulations"
date: 2016-11-04 12:10:37
permalink: /:title/
---
Ou devrais-je dire « **tribulations** » ? J'ai voulu créer un *Makefile* pour centraliser les commandes Docker que je tapais 10 fois par minutes dans mon terminal. Sauf que, [selon StackOverflow](https://stackoverflow.com/questions/16931770/makefile4-missing-separator-stop/16945143#16945143), **make* est très stupide** et s'il n'a pas des tabulations, il refusera de fonctionner. Heureusement, for heureusement, il y a [des gens géniaux](https://stackoverflow.com/questions/18936337/makefile1-missing-separator-stop/18936447#18936447) sur SO. Voilà donc un bout de code à lancer dans le terminal pour convertir les espaces en tabulations.

{% highlight shell %}
perl -pi -e 's/^  */\\t/' Makefile
{% endhighlight %}