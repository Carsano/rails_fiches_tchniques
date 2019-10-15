# ------ MVC pour une appli Ruby on Rails -----

# Contexte

Cet article fait un point sur la chronologie à suivre lors de la création du MVC pour une appli Ruby on Rails, utilisant postgresql en base de données.
Toutes les notions présentées dans cet article ont été vues, sur trois semaines, au sein de la formation The Hacking Projet.
Sans MVC, pas d'ossature pour une appli!
Il m'a paru important de les reprendres, les synthétisers, afin d'être plus efficace.

# Rappels 

Le MVC, acronyme de Model-View-Controller, est un modèle d'architecture de logiciel orienté objet, constitué de 3 grosses composantes:
- le Model, qui gère l'accès à la base de données
- la View, qui gère les interactions avec l'utilisateur
- le Controller, qui fait le rôle de chef d'orchestre de tout cela

# Principe du MVC

Chaque requête (url) que l'utilisateur envoit au routeur de l'application va appeler le Controller correspondant à l'url.
Le Controller consulte son Model relié à sa table de données.
Le Controller récupère les données et les transmet à la View correspondante pour afficher la réponse à la requête dans le navigateur.
Chaque Controller est en relation avec son Model et sa View.

# Préparation pour une appli Ruby on Rails, utilisant postgresql en gestion de base de données


### $ rails new -d postgresql nom_app
==> création de l'architecture du dossier de ton nom_app Rails

### $ cd nom_app
==> n'oublie pas de rentrer dans ton nom_app

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

# Création du Model

Les Models sont chargés de stocker des données dans des "tables".

### $ rails generate model NomModel nom_column1:type nom_column2:type ....
(type == string, integer, boolean, text, datetime,...)

## 1) Va créer ton Model NomModel dans:

	  nom_app
		|-- app 
		|   |-- models
		|   |   |-- concerns
		|   |   |-- application_record.rb
		|   |   |-- *nom_model.rb* 

Dans ton nom_model.rb, tu mettras toutes les choses importantes relatives à chaque création d'instance de ton Model

	class NomModel < ApplicationRecord
	
	   # tu peux rentrer les dépendances de ton model par rapoort aux autres, s'il y en a: 

	    ex: belongs_to :autre_model
	        has_many :autre_models, +/- through: :autre_models, +/- dependent: :destroy

	   # tu peux fixer les restrictions de tes entrées de tables avec les validates:

	      ex:  validates :nom_column1,
	           presence: true/false,
	           uniqueness: true/false,
	           length: { minimum/maximum: un nombre}
	           numericality: { greater_than: 0 }

	           validates :nom_column2,
	        		{ with: /\A[^@\s]+@([^@\s]+\.)+[^@\s]+\z/, message: "email adress please" }
	        		#email valide avec un @

	    # en cas de mot de passe crypté

	        has_secure_password (en lien avec la gem 'bcrypt')

	    # en cas de callbacks

	      ex:  after_create :methode1
	           before_save
	           before_validation

	      associer la méthode qui va avec
	          def methode1
			    		# le code de la methode1
			  		end

			  # Tu peux aussi créer des méthodes à intégrer aux validates pax ex

	end

## 2) Création, en parallèle, d'une 'migration' du Model (== table) dans /db/migrate
	
	nom_app
	|-- db
	|   |-- migrate
	|   |   |-- *numéro_de_migration_create_nom_models.rb*

Dans ta migration:

	class NomModels < ActiveRecord::Migration[5.2]
	  def change
	    create_table :nom_models do |t|
	      t.type :nom_column1 
	      t.type :nom_column2 

    # tu vas pouvoir y entrer la dépendance de ta table à d'autres, s'il y en a:

    t.belongs_to :autre_tables, index: true  
    #cette ligne rajoute la référence à la table autre_tables == autre_table_id

	      t.timestamps (crée une column id, created_at et update_at)
	    end
	  end
	end


### $ rails db:migrate
==> va "pusher" ta table en "up" == va valider les caractéristiques de ta table

### $ rails db:migrate satus
==> permet de faire un bilan sur l'état de migration de ta ou tes tables: up ou down

### Si besoin: $ rails db:rollback 
==> permet de revenir en arrière en mettant en down ta table pour la modifier

==> il faudra la "push" après avec $ rails db:migrate

Pour vérifier à quoi ressemble ta table, tu peux aller dans:

	  nom_app
		|-- db
		|   |-- migrate
		|   |   |-- numéro_de_migration_create_nom_models.rb
		|   |   |-- *schema.rb*

## WARNING : CONSULTABLE ET NON MODIFIABLE!!!

## Pour une meilleure compréhension de la construction de tes tables par des tiers et éviter de te perdre dans les migrations, il est préférable d'ajouter de nouvelles migrations pour les modifications de table plutôt que de faire des $ rails db:rollback

### $ rails generate migration NomMigration 
(ex: AddColumnToNomModel, AddIndexToNomModel)

	  nom_app
		|-- db
		|   |-- migrate
		|   |   |-- numéro_de_migration_create_nom_models.rb
		|   |   |-- *numéro_de_migration_nom_migration.rb*
		|   |   |-- schema.rb

Par exemple, dans ta nouvelle migration, tu pourras:

	class NomMigration < ActiveRecord::Migration[5.2]
		def change

		   add_column :nom_models, :nom_column, :type
		   # ajouter une nouvelle column

		ou remove_column :nom_models, :nom_column, :type
			 # supprimer une column 

		ou add_reference :nom_models, :autre_model, foreign_key: true
			 # rajoute dans la tables nom_models une column autre_model_id de la table autre_models d'un model AutreTable

		end
	end


Ex de raccourci:
### $ rails genarate migration add_nom_column_to_nom_model nom_column3:type
==> rajoute directement une nom_column3 dans ta table nom_models

N'oublie pas de "pusher" la migration:
==> $ rails db:migrate !!!!

## 3) Test en console

Une fois ta/tes tables migrées ($ rails db:migrate), teste-la/les dans la console

### $ rails console ou $ rails console --sandbox 

		- (+/- tp) n = NomModel.new(column1:type, column2:type)

		- (+/- tp) n = NomModel.save

	  - (+/- tp) n.nom_column1	

	  - (+/- tp) NomModel.all

	  - (+/- tp) n.destroy

    - (+/- tp) n.autretables (si d'autres tables, en fonction de tes liens de tables)

    - (+/- tp) n.autretable (si d'autres tables, en fonction de tes liens de tables)


### Utilisation de seeds.rb: pour créer des entrées (=objets) dans les tables

	  nom_app
		|-- db
		|   |-- migrate
		|   |   |-- numéro_de_migration_create_nom_models.rb
		|   |   |-- numéro_de_migration_nom_migration.rb
		|   |   |-- schema.rb
		|   |   |-- *seeds.rb*

  - soit à la main
	
		NomModel.destroy_all
		10.times do
			NomModel.create(column1:type, column2:type)
		end

  - soit en utilisant la gem 'faker' mise dans le Gemfile, pour créer des entrées fictives (https://github.com/faker-ruby/faker)

		require 'faker'
		NomModel.destroy_all
		10.times do
			NomModel.create(
				nom_column1: nom_du_Faker,
				nom_column2: nom_du_Faker
				)
		end

### $ rails db:seed 
==> Afin d'activer le seeds.rb

### ATTENTION: PENSE À CHANGER TON seeds.rb ET FAIRE UN $ rails db:seed A CHAQUE MODIFICATIONS DANS TES TABLES!!!!

### FAIS UN MAX DE TESTS EN CONSOLE APRÈS CRÉATION DE TES TABLES HISTOIRE DE NE PAS ÊTRE SURPRIS PAR LA SUITE!!!!

# Création des Controllers et Views

Le controller agit en tant que chef d'orchestre de l'application.
Chaque Controller possède:

  - ses méthodes reposant sur le système CRUD == ensemble d'actions que l'on peut faire sur toute instance créée par le Model

C = CREATE: créer une instance

R = READ: lire les données des instances

U = UPDATE: mise à jour des données d'une instance

D = DESTROY: supprimer une instance 

  - les méthodes sont étroitement liées aux urls envoyées par le routeur au Controller correspondant à travers le système REST

#### Le nom du Controlleur sera le même que celui du Model et de la View

#### Schéma d'imbrication de CRUD et REST

	   Routes possibles REST                                        |      méthode appelée du nom_controller      |     CRUD

	-----------------------------------------------------------------------------------------------------------------------------------

	get "/nom_controller", to "nom_controller#index"                |               index                         |     READ

	get "/nom_controller/:id", to "nom_controller#show"             |               show                          |     READ

	get "/nom_controller/new", to "nom_controller#new"              |               new                           |     CREATE

	post "/nom_controller", to "nom_controller#create"              |              create                         |     CREATE

	get "/nom_controller/:id/edit", to "nom_controller#edit"        |               edit                          |     UPDATE

	put/patch "/nom_controller/:id", to "nom_controller#update"     |              update                         |     UPDATE

	delete "/nom_controller/:id", to "nom_controller#destroy"       |              destroy                        |     DESTROY

Méthode GET: utilisée pour demander une page (et derrière la renvoyer à l'utilisateur)

Méthode POST: utilisée pour recevoir de l'information d'un utilisateur et créer une instance en base 

Méthode PUT/PATCH: utilisées pour recevoir de l'information d'un utilisateur et mettre à jour une instance en base

Méthode DELETE: utilisée pour demander la suppression d'une instance en base

### $ rails generate controller nom_controller(+s) index show new create edit update destroy

## 1) Création du Controller avec les méthodes CRUD index, show, new, create, edit, update, et destroy

	  nom_app
	  |-- app
		|   |-- controllers
		|   |   |-- concerns
		|   |   |-- application_controller.rb
		|   |   |-- *nom_controllers_controller.rb*

==> Dans app/controllers, création de nom_controllers_controller.rb 

On retrouve les méthodes annoncées lors de la création du Controller

	class NomControlers Controller < ApplicationController

	  before_action :authenticate_user, only: [:show, :new, :edit, :update, :destroy]
	  before_action :authenticate_current_user, only: [:edit, :destroy]
	  # si nécessaire, mettre les callbacks associés aux méthodes définis dans le Model correspondant 
	  
	  def index (en lien avec la route "/nom_controller", méthode GET)
	    par ex: @nommodel = NomModel.all
	  end

	  def show (en lien avec la route "/nom_controller/:id", méthode GET)
	    généralement: @nommodel = NomModel.find(params[:id])
	                  # récupère l'id de l'url via params
	  end

	  def new (en lien avec la route "/nom_controller/new", méthode GET)
	  	par ex: new user, new article,....
	  end

	  def create (en lien avec la route "/nom_controller", méthode POST)

	    par ex: @nommodel = NomModel.new(nom_column:params[:...], nom_column:.....)

	    if @nom_model.save 
	    	redirect_to prefix_route_path

	    else
	    	render 'new'
	    end
	  end


	  def edit
	  end

	  def update
	    par ex: @nommodel = NomModel.find(params[:id])

	      if @nommodel.update(column1:params[:column1])
	        redirect_to prefix_route_path
	      else
	        render :edit
	      end
	  end

	  def destroy

	      par ex: @nommodel = NomModel.find(params[:id])

	      if @nommodel.destroy 
	      	redirect_to prefix_route_path   
	      else
	        render :destroy
	      end
	  end
	end

## 2) Création des routes dans routes.rb

    nom_app
	  |-- config
		|   |-- *routes.rb*

Dans ton routes.rb:

	Rails.application.routes.draw do

	  get "/nom_controller", to "nom_controller#index"
	  get "/nom_controller/:id", to "nom_controller#show"
	  get "/nom_controller/new", to "nom_controller#new" 
	  post "/nom_controller", to "nom_controller#create"  
	  get "/nom_controller/:id/edit", to "nom_controller#edit" 
	  put/patch "/nom_controller/:id", to "nom_controller#update" 
	  delete "/nom_controller/:id", to "nom_controller#destroy"    
		
	end

### $ rails resources :nom_controllers

	Rails.application.routes.draw do

		  get "/nom_controller", to "nom_controller#index"
		  get "/nom_controller/:id", to "nom_controller#show"
		  get "/nom_controller/new", to "nom_controller#new" 
		  post "/nom_controller", to "nom_controller#create"  
		  get "/nom_controller/:id/edit", to "nom_controller#edit" 
		  put/patch "/nom_controller/:id", to "nom_controller#update" 
		  delete "/nom_controller/:id", to "nom_controller#destroy" 

		  *resources :nom_controllers*   
			
	end

Toutes les autres routes doivent être supprimées!

	Rails.application.routes.draw do
		  resources :nom_controllers  
	end

On peut ajouter des caractéristiques à resources

	Rails.application.routes.draw do

		  resources :nom_controllers, only:[:create] == le controlleur nom_controller n'appliquera que la méthode create
		  resources :nom_controllers, except:[:create] == le controlleur nom_controller appliquera toutes les méthodes sauf la méthode create

	end

Pour des imbrications de routes

==> NESTED RESOURCES

	Rails.application.routes.draw do

	  resources :controllers1 do
	   resources :controllers2
	  end

	end

==> NAMESPACE ROUTES (en cas d'espace admin par ex)

	Rails.application.routes.draw do

	  namespace :controllers1 do
	   resources :controllers2 :controllers3 .....
	  end

	end

### $ rails routes
==> permet de visualiser tes routes

### $ rails routes|grep nom_controller 
==> permet de visualiser une route spécifique

## 3) Création du View correspondant au controller 

	  nom_app
	  |-- app
	  |   |-- views
		|   |   |-- layouts
		|   |   |-- *nom_controller*
		|   |   |   |-- *create.html.erb*
		|   |   |   |-- *destroy.html.erb*
		|   |   |   |-- *edit.html.erb*
		|   |   |   |-- *index.html.erb*
		|	  |   |   |-- *new.html.erb*
		|   |   |   |-- *show.html.erb*
		|   |   |   |-- *update.html.erb*


Chaque fichier .erb correspond à la méthode CRUD du Controller associé

Tu mettras dans le fihier .erb:

  - les infos de la page (GET) en html

  - les infos que tu as besoin de récupérer (POST/PUT/PATCH/) via des formulaires

ex:

		<%= form_tag url_for(action: 'create'), method: "post" do %>
		    <%= label_tag 'Contenu' %>
		    <%= text_field_tag 'content'%>
		    <br /> <br />
		    <%= submit_tag "Crée ton gossip" %>
		<% end %>

  - tes liens entre les différentes pages de ton appli

		<%= link_to "clique ici", prefix_path %> 
		== <a href="url_liée_au_path_saisi_dans_ton_link_to">clique ici</a>


### "prefix" == prefix de la route vers lequel tu envoies ton utilisateut, visible dans $ rails routes

### A NOTER:

  - le dossier layouts contient le fichier application.html.erb, qui est le fichier mère de tous les autres fichiers .erb == permet un html commun à tous les fichiers.erb.

C'est notamment ici qu'il faudra mettre le CDN pour Bootstrap

    nom_app 
		|-- app
		|   |-- views
		|   |   |   |-- *layouts*
		|   |   |   |-- *application.html.erb*

  - Il faudra utiliser le langage ruby dans les fichiers.erb!!! 

		en mettant ton code entre <% %> + <% end %>
		en mettant <%=  %>, pour afficher son résultat dans ton html 

ex: 

		<% if @gossips != nul %>
			<% @gossips.each do |gossip| %>
						<%= image_tag("image.jpeg", :size => "319x220") %>
					  <%= link_to "#{gossip.title}", gossip_path(gossip.id)%></h5>
					  <%= gossip.user.first_name %>
			<% end %>
		<% end %>


## 4) Création des fichiers css correspondants aux fichiers.erb

    nom_app 
	  |-- app
		|   |-- assets 
		|   |   |-- stylesheets
		|   |   |   |-- application.css
		|   |   |   |-- *nom_controller.scss*

Comme pour les views, le fichier application.css est le fihier css mère.

## 5) Création des fichiers javascripts correspondants aux fichiers.erb

    nom_app 
	  |-- app
		|   |-- assets 
		|   |   |-- javascripts
		|   |   |   |-- application.js
		|   |   |   |-- *nom_controller.coffee*

Comme pour les views, le fichier application.js est le fihier javascripts mère.

# Résumé de commandes

### $ rails new -d postgresql nom_app 
==> création appli rails

### $ cd nom_app

### Gemfile 

### $ bundle install 

### rails db:create
==> création de la bdd PostgreSQL

### $ rails generate model NomModel nom_column1:type nom_column2:type ....
==> création du Model
==> création d'une migration

#### $ rails db:migrate, rails db:miragte status, rails db:rollback
==> 'push' la migration, état des migrations, 'down' la migration

#### $ rails console ou $ rails console --sandbox
==> test du Model en console

#### ($ rails generate migration NomMigration)

#### $ rails db:seed
==> crée des entrée bdd

### $ rails generate controller nom_controller(+s) index show new create edit update destroy
==> création du controller
==> création de la View correspondante

#### routes.rb
==> permet de visualiser les routes

#### $ rails routes, $ rails routes|grep 
==> idem, en console

#### Views
==> html

### css

### (js)


