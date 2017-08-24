---
layout: post
title: "Déployer sa première application Rails sur son dédié avec Passenger"
date: 2014-11-09 01:54:44 +0100
description: Tutoriel qui explique comment déployer sa première application rails sur un serveur dédié
cover: /assets/header_image.jpg
---

# Préambule

Ce post est un tutoriel destiné à expliquer comment déployer une application rails sur un dédié.
Il retrace toutes les étapes nécessaires, de la configuration des DNS à la configuration d'apache en passant par l'installation du serveur.
Ainsi, nous allons, au cours de ce tutoriel, déployer une application rails `myBlog` sur le nom de domaine `simon-ninon.fr`.

Pour que vous puissiez reproduire les différentes étapes chez vous, ce tutoriel présuppose que vous possédez votre propre nom de domaine et votre propre serveur dédié.

<!-- more -->

Enfin, afin que vous puissiez avoir quelques points de repères, voici quelques données que je vais réutiliser tout au long de ce tutoriel:

* **Nom de domaine:** simon-ninon.fr
* **Type de dédié:** Kimsufi de OVH
* **NS du serveur:** ns330962.ip-37-187-120.eu
* **IP du serveur:** 37.187.120.151
* **Serveur Web:** Apache 2
* **Serveur DNS:** Bind 9
* **OS du serveur:** Debian 7

# Préparer les DNS

La 1ère étape est de configurer les DNS de votre nom de domaine afin qu'ils pointent sur votre serveur dédié.
En effet, lorsque vous achetez un nom de domaine, les DNS sont, par défaut, configurés pour pointer sur les serveurs de votre "fournisseur".

Pour configurer cela, rien de plus simple: il suffit de vous connecter sur le manager du fournisseur chez qui vous avez acheter votre nom de domaine pour accéder à la configuration des DNS de votre nom de domaine.
Dans mon cas, il s'agit d'OVH. Si vous n'êtes pas chez OVH: pas de panique, la manipulation sera similaire quelque soit votre fournisseur. A vous de vous adapter en fonction du manager que vous aurez sous les yeux.

Ainsi, dans le cas d'OVH, une fois connecté, il vous suffit de vous rendre dans `domaine & DNS > Serveurs DNS > Modification`. Vous aurez alors un menu comme sur la photo suivante:

{% img /assets/deploy_rails_app_on_dedicated_server/ovh_manager_dns_configuration.png OVH Manager Configuration DNS %}

Vous l'aurez remarqué, dans le screenshot, j'ai configuré les DNS de mon serveur dédié:

* **DNS primaire:** ns330962.ip-37-187-120.eu
* **DNS secondaire:** ns.kimsufi.com

Le **DNS primaire** est le serveur DNS par défaut qui va être appelé pour résoudre un nom de domaine. Ainsi, lorsque j'essayerai d'accéder à `simon-ninon.fr`, mon serveur dédié sera appelé pour s'en occuper: c'est ce que l'on souhaite puisque nous allons nous occuper de la configuration de Bind (un serveur DNS) un peu plus tard dans ce tutoriel.

Le **DNS secondaire** est le serveur DNS qui sera appelé en cas d'indisponibilité du serveur DNS primaire.

Dans le cadre de ce tutoriel, nous avons fait pointer les DNS sur notre serveur afin que notre serveur s'occupe de la résolution des DNS.
Il faut cependant noter que certains fournisseurs prennent en charge la résolution des DNS gratuitement.
C'est le cas d'OVH par exemple: il vous est ainsi possible de faire pointer les DNS de votre nom de domaine vers les serveurs d'OVH et de configurer les zones DNS directement à partir de votre manager OVH. Cela revient exactement au même.

# Préparer son dédié

La 2ème étape consiste à préparer son dédié: c'est assez simple, il suffit d'installer une série de packages: apache, ruby, ...

{% codeblock lang:bash Préparer son dédié %}
# On installe apache
$ sudo apt-get install apache2

# On installe bind
$ sudo apt-get install bind9

# On installe ruby 2.1 via RVM
## installation de RVM
$ curl -sSL https://get.rvm.io | bash -s stable
## installation de ruby 2.1
$ rvm install 2.1
## on définit ruby 2.1 comme la version de ruby par défaut
$ rvm use 2.1 --default

# On installe rails
$ gem install rails --no-ri --no-rdoc

# On installe passenger
$ gem install passenger
{% endcodeblock %}

Dans le cadre de ce tutoriel, j'ai décidé d'utiliser `bind9` en tant que serveur DNS et `apache2` en tant que serveur web. Si vous avez d'autres préférences, par exemple `nginx` en tant que web serveur, n'hésitez pas.
Sachez cependant que pour faire fonctionner `phusion passenger`, vous devez avoir un serveur web `apache` ou `nginx`.

# Configurer Apache

Il est maintenant temps de s'occuper de la configuration de notre serveur web: dans mon cas `apache`.

On commence par l'installation du plugin passenger pour apache:

{% codeblock lang:bash Installation du module Passenger %}
# Dans le cas où vous utilisez apache
$ passenger-install-apache2-module

# Dans le cas où vous utilisez nginx
$ passenger-install-nginx-module
{% endcodeblock %}

Lorsque vous allez exécuter l'une de ces deux commandes, l'installateur va vérifier que vous avez bien installer tous les packets nécessaires à l'installation du module.
S'il vous manque des dépendances, pas de soucis: passenger vous indique celles qu'il vous manque et vous donne même les lignes de commande à utiliser pour les installer!

Une fois l'installation terminée, Passenger vous demande de rajouter quelques lignes à la fin de votre fichier de configuration apache. Ces lignes permettront à apache de load le module Passenger.
Ainsi, dans mon cas, je vais tout simplement rajouter à la fin du fichier `/etc/apache2/apache2.conf`:

{% codeblock lang:apacheconf /etc/apache2/apache2.conf %}
# [...]
LoadModule passenger_module /home/simon/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
  PassengerRoot /home/simon/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53
  PassengerDefaultRuby /home/simon/.rvm/gems/ruby-2.1.4/wrappers/ruby
</IfModule>
{% endcodeblock %}

Une fois le module passenger installé, il ne reste plus qu'à configurer apache pour lui indiquer dans quel dossier chercher notre application.
Pour cela, apache définit 2 dossiers:

* **/etc/apache2/site-available:** contient tous les fichiers de configuration pour chacun de vos sites webs
* **/etc/apache2/site-enabled:** contient des liens symboliques vers les fichiers de configuration des sites webs que vous souhaitez "activer"

Ainsi, dans notre cas, nous allons créé un fichier `/etc/apache2/site-available/simon-ninon.fr` avec la configuration minimale à mettre pour une application rails tournant avec passenger:

{% codeblock lang:apacheconf /etc/apache2/site-available/simon-ninon.fr %}
<VirtualHost *:80>
  ServerName simon-ninon.fr
  ServerAlias www.simon-ninon.fr
  DocumentRoot /var/www/myBlog/public
  <Directory /var/www/myBlog/public>
    AllowOverride all
    Options -MultiViews
  </Directory>
</VirtualHost>
{% endcodeblock %}

La configuration est en soit assez simple: on indique que le fichier de configuration concerne le cas où l'on reçoit une requète concernant `simon-ninon.fr` ou `www.simon-ninon.fr` et on indique que notre site se situe dans le dossier `/var/www/myBlog/public`.

Il est cependant important de noter deux détails qui sont des prérequis de passenger:

* Le `DocumentRoot` et le `Directory` pointent sur le dossier `public` de notre application.
* L'option `MultiViews` est désactivée

Il ne nous reste alors plus qu'à activer notre site web en créant un lien symbolique dans le dossier `/etc/apache2/site-enabled`:

{% codeblock lang:bash Activation de notre site %}
# Création du lien symbolique
$ ln -s /etc/apache2/site-available/simon-ninon.fr /etc/apache2/site-enabled
{% endcodeblock %}

Maintenant que notre serveur web est configuré, il ne reste plus qu'à le redémarrer pour appliquer les changements:

{% codeblock lang:bash Redémarrage d'apache %}
$ sudo /etc/init.d/apache2 restart
{% endcodeblock %}

# Configurer Bind

Partie un peu plus délicate maintenant: la configuration du serveur DNS `bind9`.
La configuration d'un serveur DNS est loin d'être aussi simple que celle d'un serveur web comme apache: ce n'est pas grave si vous ne saisissez pas toutes les explications qui vont suivre.

Nous devons tout d'abord définir une "zone", c'est-à-dire un ensemble de nom de domaines sur lequel le serveur a autorité.
Pour cela, il nous suffit de modifier le fichier `/etc/bind/named.conf.local` et d'y créer notre zone.

{% codeblock lang:bash /etc/bind/named.conf.local %}
zone "simon-ninon.fr" {
  type master;
  file "/etc/bind/zones/simon-ninon.fr.db";
};
{% endcodeblock %}

Ici, on crée une zone nommée `simon-ninon.fr` de type `master` (à différencier de `slave`).
Enfin, on indique que la configuration de la résolution des noms de domaine se trouve dans le fichier `/etc/bind/zones/simon-ninon.fr.db`.

Comme vous vous en doutez, nous devons donc maintenant créer le fichier `/etc/bind/zones/simon-ninon.fr.db` afin de permettre à bind de fonctionner correctement.

{% codeblock lang:bash /etc/bind/zones/simon-ninon.fr.db %}
$TTL 3h
@ IN SOA simon-ninon.fr. simon.simon-ninon.fr. (2014110301 8H 2H 1W 1D)
	IN	A	37.187.120.151
	IN	NS	simon-ninon.fr.
www	IN	CNAME	simon-ninon.fr.
{% endcodeblock %}

Le contenu de ce fichier peut paraître perturbant au 1er abord. Je vais essayer de le clarifier ligne par ligne:

1. C'est le TTL, comprenez `Time To Live`. Cela correspond à la durée de validité de cette configuration permettant aux serveurs intermédiaires de s'avoir s'ils doivent rafraîchir leurs informations.
2. Ici, on décrit les paramètres globaux de la zone (type `SOA`, à savoir `Start of Authority`). On doit fournir un DNS (`simon-ninon.fr.`) et l'adresse email du responsable de la zone (`simon.simon-ninon.fr.`). Enfin, entre parenthèses, on fournit d'autres informations annexes: un serial number, un temps de refresh pour les slaves, un temps de retry dans le cas où le refresh n'a pas abouti, une durée d'expiration et le nxdomain TTL.
3. Cette ligne est particulièrement simple: le type `A` permet d'établir la correspondance entre notre nom de zone et l'IP de notre serveur `37.187.120.151`.
4. Ici, on indique quel est le serveur DNS faisant autorité (type `NS`).
5. Enfin, à la dernière ligne, nous créons un alias pour le sous-nom de domaine `www` (type `CNAME`). Si vous voulez paramétrer d'autres sous-nom de domaines, c'est donc ici que vous pouvez le faire.

Enfin, afin que notre serveur DNS puisse fonctionner, il faut aboslument commenter deux lignes dans le fichier `/etc/bind/named.conf.options`:

{% codeblock lang:bash /etc/bind/named.conf.options %}
   #listen-on-v6 { ::1; };
   #listen-on { 127.0.0.1; };
{% endcodeblock %}

En effet, si vous ne commentez pas ces lignes, votre serveur DNS ne sera réceptif qu'aux requètes locales mais pas aux requètes extérieurs... C'est loin d'être ce que l'on souhaite!

Notre serveur DNS est donc normalement configuré, il ne reste plus qu'à le redémarrer pour appliquer les changements:

{% codeblock lang:bash Redémarrage de bind %}
$ sudo /etc/init.d/bind9 restart
{% endcodeblock %}

N'hésitez pas à vérifier que votre configuration a pu être chargée sans soucis: la moindre erreur provoquera un dysfonctionnement de bind.
Pour cela, allez voir le fichier `/var/log/syslog`. Il vous tiendra au courant en cas d'erreur (ou de succès évidemment).

Sachez cependant que la plupart du temps, les erreurs proviennent de petit détails:

* Oubli d'un `.` après un nom de domaine absolu
* Oubli d'un type `NS` ou `A`
* Format de l'adresse mail de contact erronée 

Vous avez également des outils à votre disposition pour vérifier votre configuration:

* La commande `host`
* La commande `named-checkconf`

# Déployer son application

Notre serveur est maintenant prêt: le serveur web et le serveur DNS sont configurés comme il faut. Il ne nous reste donc plus qu'à déployer notre application au bon endroit:

{% codeblock lang:bash Déployement de l'application %}
# On crée notre application rails
$ cd ~
$ rails new myBlog

# On installe les dépendances
$ cd myBlog
$ bundle install

# On fait en sorte qu'apache puisse la trouver
$ sudo ln -s ~/myBlog /var/www
{% endcodeblock %}

Ca y est, notre application est disponible sur notre nom de domaine `simon-ninon.fr`!

Sachez que passenger tentera, par défaut, d'écrire les logs dans `/path/to/your/app/log/production.log`. S'il n'y arrive pas, vous pourrez les retrouver dans les fichiers de logs apache (dans `/var/log/apache2`).
Ainsi, si vous avez une erreur après le déployement, n'hésitez pas à vous y référer afin de savoir ce qui ne va pas (par exemple, l'absence d'un javascript runtime tel qu'`execjs`).

{% include about_simon_ninon.markdown %}

