---
layout: post
title: "Suivre la production avec Errbit"
date:  2015-03-27 20:54:44 +0100
description: Tutoriel qui explique comment installer Errbit, un puissant outil pour verifier la production d'une application Rake
cover: /assets/header_image.jpg
tags: rails french
---

# Késako?

Errbit est un outil permettant de tracker et de gérer les erreurs survenant sur une application.
Le cœur du service est une API compatible avec airbrake.

L'intérêt est alors d'installer Airbrake sur son application Rails. Cette gem va détecter les erreurs survenant en production et va notifier l'API d'errbit.

<!-- more --> 

En fonction de la configuration d'errbit, ce dernier va alors envoyer des mails à la "mailing-list" de l'application et va fournir une gestion de l'erreur sur une interface web (l'erreur est-elle traitée? faut-il ouvrir un ticket github? Combien de fois l'erreur est-elle survenue? Quelle est la callstack? ...).

<img src="/assets/suivre_la_production_avec_errbit/dashboard.png" title="Errbit Dashboard"/>

L'installation de la Gem se divise donc en 2 parties:

* L'installation de l'API/Service web
* L'installation d'airbrake sur l'application rails

# Installation API - Service Web

## MongoDB

Il faut dans un premier temps installer mongodb qui est utilisé par errbit pour stocker les données.
Les étapes à suivre en fonction de chaque OS sont disponibles [ici](http://docs.mongodb.org/manual/installation/)

Dans le cas de debian:
{% highlight bash %}
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10 # On récupère la clé publique
$ echo 'deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list # On ajoute le dépot
$ sudo apt-get update # On met à jour la liste des dépots
$ sudo apt-get install -y mongodb-org # On installe mongoDB
$ sudo service mongod start # On démarre mongoDB
{% endhighlight %}

Quelque soit l'OS, et pour les besoins d'errbit, on enchaîne ensuite avec:

{% highlight bash %}
$ sudo apt-get install mongodb-10gen
{% endhighlight %}

## Dépendances

Errbit nécessite l'installation de quelques libs pour pouvoir fonctionner:

{% highlight bash %}
$ sudo apt-get install libxml2 libxml2-dev libxslt-dev libcurl4-openssl-dev libzip-dev libssl-dev
{% endhighlight %}

## Errbit

Une fois toutes les dépendances installées, il est maintenant possible de déployer Errbit.

{% highlight bash %}
$ git clone https://github.com/errbit/errbit # On clone les sources d'errbit
$ git pull origin master # Pour récupérer les dernières mises à jours, on pull (ce n'est donc pas nécessaire après un clone)
$ bundle install # On installe les dépendances
$ RAILS_ENV=production rake errbit:bootstrap # On seed la DB
$ RAILS_ENV=production rake assets:precompile # On précompile les assets
$ RAILS_ENV=production rails server # On démarre le serveur (ça peut être autre chose, passenger par exemple)
{% endhighlight %}

Bien évidemment, `RAILS_ENV` est à définir en fonction de l'environnement dans lequel errbit va être lancé.

La commande `rake errbit:bootstrap` est censée seed la base de donnée avec un utilisateur par défaut.
Il peut être intéressant de vérifier si cela s'est bien déroulé via la console rails.
Si aucun utilisateur n'a été créé, cela peut venir de:

* l'environnement dans lequel la têche a été lancée (utilisateur créé sur l'environnement de production mais application lancée en développement)
* mongoDB qui a mal été installé

Les informations sur l'utilisateur créé sont situés dans le fichier `db/seeds.rb`.

## Configuration

Une fois errbit installée, il est possible de le configurer simplement via des variables d'environnements.

La liste complète des variables d'environnement que l'on peut setter sont disponibles sur la [documentation d'errbit](https://github.com/errbit/errbit/blob/master/docs/configuration.md).

Il suffit alors simplement de setter les variables qui nous intéressent dans un fichier `.env` à la racine de l'application:

{% highlight ruby %}
# Configuration du hostname
ERRBIT_HOST = "errbit.simon-ninon.fr"
# Configuration du mailer
ERRBIT_EMAIL_FROM = "errbit@simon-ninon.fr"
# Configuration de l'API Github
GITHUB_CLIENT_ID = "some_github_client_id"
GITHUB_SECRET = "some_github_secret"
# Configuration du key secret
SECRET_KEY_BASE = "some_secret_key_base"
{% endhighlight %}

Tout changement de configuration nécessite un redémarrage du serveur Rails.

# Installation d'airbrake sur l'application Rails

## API Key

Une fois le service web installé et configuré, il est temps d'associer notre application Rails pour qu'elle notifie errbit (qui pourra ainsi lui même nous notifier en cas de problèmes sur la production).

Pour cela, il faut dans un premier temps créer une application sur l'application Errbit. Cela nous générera une application API key que l'on pourra réutiliser dans notre application Rails.

<img src="/assets/suivre_la_production_avec_errbit/create_app.png" title="Créer une app Errbit"/>

## Airbrake

Une fois l'application créée et configurée sur la plateforme Errbit, il est alors possible de configurer notre application Rails.

Pour que celle-ci puisse détecter les crash et puisse notifier l'API d'errbit avec le bon format de requètes, il faut passer par la gem Airbrake.

Pour cela, il suffit dans un premier temps d'ajouter cette gem au Gemfile (`gem 'airbrake'`) et de l'installer (`bundle install`).

## Configuration

Il est alors nécessaire de configurer aibrake. Cela se passe dans un initializer, `config/initializers/errbit.rb` par exemple.

Il est alors important de comprendre qu'il existe différentes versions de l'API airbrake: v1, v2, v3.
Errbit les supporte toutes et saura quelle API est utilisée en fonction du format de la requête.

Pour utiliser l'API v2:

{% highlight ruby %}
Airbrake.configure do |config|
  config.api_key = 'api_key'
  config.host    = 'host'
  config.port    = 80
  config.secure  = config.port == 443
end
{% endhighlight %}

Pour utiliser l'API v3:

{% highlight ruby %}
Airbrake.configure do |config|
  config.api_key 	= 'api_key'
  config.host    	= 'host'
  config.port    	= 80
  config.secure  	= config.port == 443
  config.project_id	= 'api_key'
end

class Airbrake::Sender
  def json_api_enabled?
    true
  end
end
{% endhighlight %}


## Vérifier si ça fonctionne

Pour vérifier si ça fonctionne, rien de plus simple: il suffit de forcer la production à planter.
Si tout fonctionne, une erreur devrait apparaitre sur l'interface web d'Errbit et un mail devrait être envoyé à chaque membre de la mailing list.

Si ce n'est pas le cas, il est possible d'en savoir plus sur la raison de l'erreur à travers les logs d'errbit (`tail -f log/production.log`). Le problème peut venir de différentes sources:

* Mauvaise API_KEY (invalide, périmée, ...)
* Version de l'API mal gérée (il peut être intéressant de tester l'API v3 si ça ne marche pas avec la v2 et inversement).

<img src="/assets/suivre_la_production_avec_errbit/app_errors.png" title="Liste d'erreurs d'une app"/>

<img src="/assets/suivre_la_production_avec_errbit/error_summary.png" title="Détails d'une erreur"/>

<img src="/assets/suivre_la_production_avec_errbit/error_backtrace.png" title="Callstack d'une erreur"/>

## Déploiement

Il est possible de notifier Errbit lorsqu'une nouvelle version de l'application est disponible.

Les différentes configurations sont disponibles [ici]().

Dans le cas de capistrano, il suffit d'ajouter la ligne `require 'airbrake/capistrano3'` dans le `Capfile`.
Cela va créer de nouvelles tasks capistrano gérant cette notification.

