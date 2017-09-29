---
layout: default
title: "Clic-droit et JavaScript"
date: 2016-12-07 10:12:43
permalink: /:title/
---
Encore aujourd'hui, il existe des sites qui interdisent le clic droit, pour je ne sais quelle raison. Gêner un utilisateur qui voulait copier un bout de phrase ? L'empêcher de « voler » une photo , quand bien même on peut faire une capture d'écran ? Je ne sais pas. Mais moi je suis codeur et je sais modifier du JavaScript. Et bien souvent, une simple ligne de code comme ci-dessous, dans la console (dont l'accès dépend du navigateur), suffit.

{% highlight javascript %}
document.oncontextmenu = new Function("return true");
{% endhighlight %}

Ou pire : désactiver JavaScript. Ça me rappelle un commitstrip tient. Ah, qu'il est bon d'être codeur !

![Des codeurs tapent une personne qui leur interdisait d'utiliser le clic-droit.](http://www.commitstrip.com/wp-content/uploads/2016/06/Strip-Les-codeurs-et-les-images-650-final-1.jpg)](http://www.commitstrip.com/wp-content/uploads/2016/06/Strip-Les-codeurs-et-les-images-650-final-1.jpg)