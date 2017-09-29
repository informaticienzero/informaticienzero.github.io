---
layout: default
title: "MTP sous Archlinux"
date: 2016-07-18 19:06:53
permalink: /:title/
---
Un petit billet rapide aujourd'hui, qui nous changera un peu de la série d'articles sur les 97 connaissances du programmeur et qui me servira de mémo. Nous allons parler de comment connecter un téléphone Android, en l’occurrence un OnePlus One en MTP sous Archlinux. C'est tout simple. Il suffit de créer un répertoire et de monter le téléphone dessus.

{% highlight shell %}
mkdir ~/mnt
jmtpfs ~/mnt
{% endhighlight %}