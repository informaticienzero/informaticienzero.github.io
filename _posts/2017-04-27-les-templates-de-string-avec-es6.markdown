---
layout: default
title: "Les templates de string avec ES6"
date: 2017-04-27 22:55:54
permalink: /:title/
---
Je suis un grand débutant en JavaScript, mais je tente d'utiliser un maximum la dernière version du langage, de même que je remplace mon utilisation de jQuery basique par du VanillaJS.

Sauf que je n'arrivais pas à faire fonctionner les *strings templates*. Mes variables n'étaient pas parsées pour la simple et bonne raison qu'il ne faut pas utiliser les guillemets simples `'``  mais les caractères accent grave. C'est tout bête, surtout que y'avait un exemple dans le code, sous mes yeux. Mais c'est comme ça quand on débute.

{% highlight javascript %}
mainWindow.loadURL(`file://${__dirname}/app/html/index.html`);
{% endhighlight %}

L'essentiel, c'est que **je m'éclate avec Nodejs et Electron** !
