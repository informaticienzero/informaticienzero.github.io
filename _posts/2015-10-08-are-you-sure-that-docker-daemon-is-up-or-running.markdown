---
layout: default
title: "Are you sure that Docker daemon is up or running ?"
date: 2015-10-08 20:53:58
permalink: /:title/
---
Il y a quelques jours, j'ai voulu installer [Docker](https://www.docker.com/), une technologie de virtualisation légère se basant sur le système GNU/Linux. J'ai donc suivi la documentation officielle, mais je rencontrai une erreur quand je tentais de le lancer depuis mon compte, alors que tout fonctionnait parfaitement en root.

C'est grâce au site [askubuntu](https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo/477554#477554) que j'ai pu trouver la solution. Je la réécris en dessous pour la postérité.

*   Créér le groupe *docker* s'il n'existe déjà :

{% highlight shell %}
sudo groupadd docker
{% endhighlight %}

*   Ajouter l'utilisateur voulu à ce groupe :

{% highlight shell %}
sudo gpasswd -a ${USER} docker
{% endhighlight %}

*   Redémarrer le daemon Docker :

{% highlight shell %}
sudo systemctl restart docker
{% endhighlight %}

*   Se déconnecter puis se reconnecter, ou entrer la commande suivante pour mettre les groupes à jour :

{% highlight shell %}
newgrp docker
{% endhighlight %}