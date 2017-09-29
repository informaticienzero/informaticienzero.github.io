---
layout: default
title: "Installer les mises à jour Windows"
date: 2016-12-01 08:30:29
permalink: /:title/
---
On peut se dire ce qu'on veut, Windows s'utilise très majoritairement avec son interface graphique. Mais voilà, parfois, il faut la ligne de commande pour se faciliter la vie. La situation résumée : je veux installer les mises à jour de Windows 7 dans une VM au travail, mais les administrateurs systèmes **bloquent Windows Update** pour choisir quelle mise à jour installer et quand.

<!--excerpt-->

J'ai résolu mon premier problème avec un excellent logiciel du nom de **[Windows Updates Downloader](http://www.windowsupdatesdownloader.com/)**, qui permet même de sélectionner celles que l'on aimerait récupérer (sécurités, optionnelles, .NET, etc). Mais bon, installer 350+ mises à jour à la main, en double-cliquant puis en redémarrant après (presque) chaque installation, **c'est trop lourd** !

C'était sans compter sur [ce blog](https://harshilsharma63.wordpress.com/2014/12/27/how-to-install-multiple-msu-windows-update-files-with-powershell-script/). J'ai repris son script en l'améliorant un peu. Pour accélérer grandement le script, **j'ai désactivé la Restauration**. À vous de voir si vous courrez le risque ou non d'enchaîner les mises à jour sans une seule sauvegarde intermédiaire. Également, [j'ai désactivé Windows Update](http://www.msfn.org/board/topic/162714-batch-script-to-install-updates-takes-forever/#comment-1037865) (Panneau de configuration -> Windows Update -> Modifier les paramètres -> Ne jamais rechercher de mises à jour).

{% highlight powershell %}
$dir = (Get-Item -Path ".\\" -Verbose).FullName
Foreach($item in (ls $dir *.msu -Name | sort -property LastWriteTime))
{
	echo $item
	$item = $dir + "\\" + $item
	net stop wuauserv
	wusa $item /quiet /norestart | Out-Null
}
{% endhighlight %}