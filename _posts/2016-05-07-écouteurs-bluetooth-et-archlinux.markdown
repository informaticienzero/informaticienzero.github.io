---
layout: default
title: "Écouteurs Bluetooth et Archlinux"
date: 2016-05-07 22:51:24
permalink: /:title/
---
J'ai une paire d'écouteurs bluetooth Mpow Swift et un PC tournant sous Archlinux. J'ai donc naturellement envie de les connecter. Seulement voilà, tout se passe bien mais la qualité est vraiment toute pourrie. Pourquoi ? Parce que la qualité est fixée sur **Headset Head Unit** et que je ne peux passer à **Higth Fidelity Pack (A2DP Sink)**.

<!--excerpt-->

La solution a été postée [ici](https://bbs.archlinux.org/viewtopic.php?pid=1526534#p1526534) mais je la remets pour la postérité.

*   Éditer le fichier */etc/pulse/default.pa* et commenter la ligne *load-module module-bluetooth-discover* en ajoutant un* #* devant.
*   Éditer le fichier */usr/bin/start-pulseaudio-x11* pour y mettre ce code.

{% highlight bash %}
# Ligne déjà existante
if [ x”$SESSION_MANAGER” != x ] ; then
		/usr/bin/pactl load-module module-x11-xsmp “display=$DISPLAY session_manager=$SESSION_MANAGER” > /dev/null
	fi

# Rajouter cette ligne là.
/usr/bin/pactl load-module module-bluetooth-discover
{% endhighlight %}

*   Redémarrer et profiter d'un son de qualité.

 
