
# création de l'appli rails

### $ rails new -d postgresql nom_app
==> création de l'architecture du dossier de ton nom_app Rails

### $ cd nom_app
==> n'oublie pas de rentrer dans ton nom_app

### Créer repo github et brancher l'app

### Gemfile
==> mets tes gems préférées, genre:
	- gem 'devise'
  - gem 'pry'
  - gem 'table_print'
  - gem 'faker'
  - gem 'letter_opener'
  - gem 'jquery-rails'

### $ bundle install
==> installation des gems

### $ rails db:create 

==> création de ta base de données PostgreSQL

### Devise ==> penser à mettre en place Devise en premier lieu pour éviter de modifier la migration créée par Devise concernant le Model User 

# Si user et user/admin, pour interface admin, mettre cash une colonne admin dans la table users fixée à False

### Brancher Action_mailer

### pour moyen de paiement par carte: brancher Stripe

### pour Html == 3 possibilités

==> kit ui

==> Bootstrap

==> Template Bootstrap ou pas

