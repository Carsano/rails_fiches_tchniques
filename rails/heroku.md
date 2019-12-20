
# Préparation

## Si envoi de mails Sengrid ou autre, configurer pour la production

ex:

config.action_mailer.perform_caching = false

config.action_mailer.show_previews = true

config.consider_all_requests_local = true

config.action_mailer.default_url_options = { :host => 'point-budget.herokuapp.com' }

==> cf. action_mailer.md

## si envoi de mail suite à un contact_form

	config.action_mailer.default_url_options = { :host => 'new_app_name.herokuapp.com' }  
	config.action_mailer.delivery_method = :smtp  
	config.action_mailer.perform_deliveries = true  
	config.action_mailer.raise_delivery_errors = false  
	config.action_mailer.default :charset => "utf-8"  
	config.action_mailer.smtp_settings = {
	  address:              'smtp.gmail.com',
	  port:                 587,
	  domain:               'new_app_name.herokuapp.com',
	  user_name:            ENV["GMAIL_EMAIL"],
	  password:             ENV["GMAIL_PASSWORD"],
	  authentication:       'plain',
	  enable_starttls_auto: true  }

## Asset pipeline

### $ rails assets:clean assets:precompile

==> Elle aura pour effet de faire le ménage dans les assets que n'utilises plus, puis de compiler les assets pour la production.

==> Dans environnements/production.rb

 config.assets.compile = true

==> cf asset_pipeline.md 

## Active Storage(dans developpements/production.rb)

==> config.active_storage.service = :local

On peut tester en prod pendant 24h

==> config.active_storage.service = :service-choisi

cf. active_storage.md


# AVOIR COMMITÉ et PUSHER sur github !!!!

$ git add .

$ git commit -m ""

$ git push origin master

(## $ heroku create app-name (sauf si création sur heroku)

==> crée une application sur Heroku, lié à l'application

==> changer le nom de l'app 

$ heroku apps:rename new-app-name)


# Préparation Heroku.com 

==> create a new pipeline: remplir

- Pipeline name
- Connect to Github tout de suite

Create pipeline

# Heroku-App-Staging

==> Staging

 - Create a new app
 - App name
 - Choose a region
 - Pipeline stage == staging
 - Create app

 - Overview: installer add-ons Postgresql + autres
 - Resources: verif add-ons
 - Deploy: clicker sur enable automatic deploy
 - Settings: config vars == entrer les clé API
 						 add buildpack == heroku/ruby


## $ git remote add heroku <url remote staging>

==> création de la remote heroku-staging

## git remote --v ou git config --list | grep heroku

==> permet de voir les remotes git et heroku

## git remote set-url heroku <remote heroku name>

==> permet de changer la remote heroku

## $ git remote rm <remote name> 

==> supprimer une remote


## En team 

- le but sera de créer une branche develoment ou staging afin d'éviter de push sur master

- créer une work-branch

- pusher la work-branch sur github

- faire une pull request pour merger la work-branch sur development ou staging

- faire une pull request pour merger development ou staging sur master

### $ git checkout -b staging

==> création de la branche staging et on se met dessus cash

### $ git checkout -b work-branch

==> effectuer les changements

==> $ git commit work_branch

### $ git checkout staging

### $ git pull origin staging

### $ git checkout work_branch

### $ git merge staging + gestion des conflits

### $ git push origin work_branch

### Github pull request pour merger sur staging

### Github pull request pour merger staging sur master


## En solo

Prépa master

### $ git push origin master

Prépa branche staging

### $ git branch staging

### $ git push origin staging

Pour chaque travail

### $ git checkout staging

### $ git pull origin staging

### $ git checkout -b work-branch

==> git merge staging

==> effectuer les changements

==> $ git commit work_branch

### git push origin work-branch

Github 

### Faire les pull request pour merge workbranch sur staging, puis staging sur master


# $ heroku login 

==> permet de se connecter au bon compte heroku

# $ git push heroku master

==> push la remote sur heroku-staging

==> Cliquer sur Open app


## Si pb

==> $ heroku logs ou $ heroku --tail

Permet de voir les pb à régler si rien ne s'affiche!!!

==> $ heroku restart 

## $ heroku run bundle install


## Si db

### $ heroku run rails db:migrate

### +/- $ heroku run rails db:seed (si seed présent)

### +/- $ heroku run rails console


## Si modification db 

### $ heroku restart

### $ heroku pg:reset DATABASE

==> reset les tables 

### $ heroku run rails db:migrate

### +/- $ heroku run rails db:seed (si seed présent)

### +/- $ heroku run bundle install


# Heroku-App-Production

==> Production

 - Create a new app
 - App name
 - Choose a region
 - Pipeline stage == production
 - Create app

 - Overview: installer add-ons idem staging
 - Settings: config vars == idem clés API ....
 						 add buidlpacks = idem staging

## Clicker sur Promote to production

## $ heroku run bundle install -a mon-app-prod


## Si db !!!!

### $ heroku run rails db:migrate -a mon-app-prod

### $ heroku run rails db:seed -a mon-app-prod

### $ heroku pg:reset DATABASE -a mon-app-prod


## Si modification db 

### $ heroku restart -a mon-app-prod

### $ heroku pg:reset DATABASE -a mon-app-prod

==> reset les tables 

### $ heroku run rails db:migrate -a mon-app-prod

### +/- $ heroku run rails db:seed -a mon-app-prod (si seed présent)


### $ git push --force heroku master 

forcer git push heroku master, notamment quand on a dû forcer git push origin master


# Plusieurs environnements de production ou staging

https://medium.com/dev-on-rails/environnement-de-staging-et-pipeline-de-production-sur-heroku-7fb98ce32ab1


ex: j'ai une app-staging branché sur github master et une app-prod

Je veux tester une nouvelle fonctionnalité et je ne veux pas la tester sur app-staging

## création d'une nouvelle app dans la section staging de heroku => ex: app-test

==> mettre les addons nécessaires

==> +/- clés utiles

==> branché cette nouvelle appli à la branch de test github

## git et github

==> pusher la branch test sur github

## $ git remote add test-heroku <remote heroku name>

==> création d'une nouvelle remote heroku

## $ git remote --v

==> vérification de l'ajout de la remote test

## $ heroku login

==> pour se connecté au compte heroku de l'appli

## $ git push test-heroku branch_test_name:master

==> on push sur la remote test-heroku la branch test en master

## Si db et/ou seed

### $ heroku run rails db:migrate -a app-test

### $ heroku run rails db:seed -a app-test

## $ heroku restart -a app-test










