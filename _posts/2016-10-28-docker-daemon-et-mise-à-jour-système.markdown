---
layout: default
title: "Docker daemon et mise à jour système"
date: 2016-10-28 14:10:33
permalink: /:title/
---
Aujourd'hui, Docker m'a fait une jolie frayeur quand j'ai lancé un build. Subitement, il plantait à l'installation des packages en m'affichant une erreur pour le moins étrange, un peu dans le style de la suivante.

{% highlight shell %}
Failed sandbox add: failed to add interface [container ID] to sandbox.
Failed to create endpoint [container name] on network bridge.
{% endhighlight %}

En fait, la solution (encore une fois grâce à [StackOverflow](https://stackoverflow.com/questions/29546388/getting-an-operation-not-supported-error-when-trying-to-run-something-while-bu/29546560#29546560) est toute bête : je venais de mettre mon Archlinux à jour et le daemon Docker a du s'emmêler les pinceaux quelques part. Un simple redémarrage a suffit. Comme dit le proverbe, « **dans le doute, *reboot*** ».