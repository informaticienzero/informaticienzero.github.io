---
layout: default
title: "Calculer l'âge exact en Python"
date: 2016-10-08 12:18:16
permalink: /:title/
---
Hier, pour un projet que je fais en Python (dont je parlerai bientôt, en fonction des résultats), je voulais **afficher mon âge exact** en fonction de la date d'aujourd'hui, sous la forme *x* années, *y* mois et *z* jours. J'étais parti dans des calculs compliqués (année bissextile ou pas, combien de jours entre tel mois et tels mois, etc) et finalement la solution m'est apparue tout simplement après manger (comme quoi, ça aide de manger). Il suffit d'additionner son nombre de jours par rapport au 1 janvier de l'an 1 puis retirer 1 à l'année, au mois et au jour.

<!--excerpt-->

{% highlight python %}
from datetime import date, timedelta

def age(birth: date) -> (int, int, int):
		"""
			Get a 3-int tuple telling the exact age based on birth date.
		"""
		today_age = date(1, 1, 1) + (date.today() - birth)
		return (today_age.year - 1, today_age.month - 1, today_age.day - 1)
{% endhighlight %}

Par contre, j'ai une pensée émue pour les développeurs qui codent ces modules de date, de temps, de localisation, etc. C'est vraiment **un énorme boulot** pour nous faciliter la vie à nous, utilisateurs des langages et frameworks. Il n'y a quasiment aucune chance que l'un d'eux me lisent un jour, mais **vraiment un grand merci** !
