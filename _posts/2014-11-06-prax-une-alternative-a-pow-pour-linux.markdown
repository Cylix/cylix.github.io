---
layout: post
title: "Prax: une alternative à Pow pour linux"
date: 2014-11-06 02:58:15 +0100
description: Présentation d'une alternative à Pow, un serveur d'applications rack, pour linux.
cover: /assets/header_image.jpg
categories: rails french
author: Simon Ninon
---

# Vous avez dit pow?

Pow est un serveur d'applications rack qui a la particularité de ne nécessiter aucune configuration.
Pow est ainsi très pratique puisqu'il permet de lancer plusieurs applications rack en local sans se soucier de problématiques liées à la configuration de serveur.

<!-- more -->

Par ailleurs, son utilisation est particulièrement simple:

{% highlight bash %}
$ cd mon_application_rack

# on ajoute mon_application_rack sous le contrôle de pow
$ powit

# Une autre manière d'ajouter mon_application_rack sous le contrôle de pow
$ ln -s /path/to/mon_application_rack ~/.pow
{% endhighlight %}

Ainsi, mon application devient accessible en local à l'URL `mon_application_rack.dev`. Il n'y a pas besoin de s'embêter à démarrer son serveur ou à gérer un port de connexion par exemple: Pow se charge de tout!

En savoir plus sur pow:

* [Github](https://github.com/basecamp/pow)
* [Site officiel](http://pow.cx/)


# Et Prax dans tout ça?

Pow s'avère donc particulièrement utile et efficace. Cependant, il possède une limite, et pas des moindres: il n'est fonctionnel que sous Mac OS X.
Et bien malheureusement, les équivalents sous linux sont quasiment inexistants... Mais c'est son compter sur Prax!

Prax est un véritable Pow pour Linux. On retrouve ainsi un fonctionnement tout à faire similaire à celui de Pow:

{% highlight bash %}
$ cd mon_application_rack

# On ajouter mon_application_rack sous le contrôle de prax
$ prax link

# Une autre manière d'ajouter mon_application_rack sous le contrôle de prax
$ ln -s /path/to/mon_application_rack ~/.prax
{% endhighlight %}


Et tout comme avec Pow, notre application devient disponible à l'URL `mon_application_rack.dev`!

En revanche, il faut tout de même noter quelques faiblesses de Prax:

* Prax semble un peu plus instable que pow. J'ai notamment au quelques soucis au niveau du démarrage de prax (et donc de l'accessibilité des applications).
* Prax peut s'avérer beaucoup plus lent que pow.

Prax n'en demeure pas moins la meilleure alternative actuelle à Pow que je connaisse et facilite énormément le développement d'applications rack sous linux!

En savoir plus sur Prax:

* [Github](https://github.com/ysbaddaden/prax)
