---
layout: default
title: "Docker Daemon sur EC2"
date: 2016-10-27 12:03:22
permalink: /:title/
---
Hier, en déployant une application sur Elastic Beanstalk, j'ai voulu accéder en SSH à mon conteneur Docker pour lire les logs de Gunicorn et nginx, notamment. Sauf qu'en lançant un `docker ps` pour trouver l'ID de mon conteneur, j'ai obtenu une jolie erreur.

{% highlight shell %}
$ docker ps Cannot connect to the Docker daemon. Is the docker daemon running on this host?
{% endhighlight %}

La solution est très simple et [fournie sur ce blog](https://blog.cloudinvaders.com/connect-to-docker-daemon-on-aws-beanstalk-ec2-instance/). Il suffit d'ajouter l'utilisateur `ec2-user` au groupe `docker` et se déconnecter / reconnecter.

{% highlight shell %}
sudo gpasswd -a ec2-user docker
{% endhighlight %}