---
layout: default
title: "Les malheurs de GitLab"
date: 2017-02-02 09:19:09
permalink: /:title/
---
[GitLab](https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/), un concurrent de GitHub, que j'utilise personnellement pour ses dépôts privés gratuits, a subi une panne assez problématique, puisqu'elle a entraîné une perte de données. Pourtant, les admins systèmes de chez GitLab sont certainement des pros qualifiés, et ils avaient mis en place des moyens de protection. Je cite [ZdS](https://zestedesavoir.com/forums/sujet/7905/gitlabcom-ou-de-limportance-de-verifier-ses-sauvegardes/).

<!--excerpt-->

> Un employé s'est planté de serveur, et a supprimé la copie fonctionnelle de la base de prod. Oups. Que faire ?
> 
> 1.  C'est pas grave, il y avait des snapshots LVM automatiques. Mais non, parce qu'ils n'étaient pris que toutes les 24 heures (heureusement il y en avait un manuel de « seulement » 6 heures plus tôt). Et 24 heures de pertes pour ce genre de données, c'est trop.
> 2.  Mais c'est pas grave, il y avait aussi les backups normaux. Mais en fait non : ils étaient HS et personne ne s'en était rendu compte.
> 3.  Mais c'est pas grave, parce qu'ils y avait des dumps SQL. Sauf que les dumps échouaient silencieusement à cause d'une erreur de configuration que personne n'a vu.
> 4.  Mais c'est pas grave, tout ça tourne sur du Azure, avec des snapshots des disques NFS. Sauf pour les disques des SGBD, vérification faite.
> 5.  Mais c'est pas grave, parce que tout ça est sauvegardé sur du Amazon S3. Dont le compte est vide, en réalité.
> 
> 5 systèmes de sauvegarde / tolérance de pannes ; les 5 étaient HS simultanément. Joli score non ?

J'ai vraiment une pensée émue pour les admins de GitLab. Qui d'entre nous n'a jamais galéré pour un simple PC en panne qui nous a pris 3h à réparer ? Là, on parle quand même d'un gros service, avec des conséquences lourdes derrière. Je ne regarderai plus un crash de PC personnel de la même façon après ça. Comme quoi, il y a toujours pire que soi.