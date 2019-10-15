
# -------------- ActiveStorage -----------------

Active Storage est une fonctionnalité sympathique de Rails qui permet à un utilisateur de votre app de mettre en ligne des fichiers qui pourront ensuite être utilisés dans son site web. L'exemple typique est le fait d'envoyer une photo de profil : celle-ci est alors stockée dans le cloud et l'app Rails peut l'appeler à tout moment afin de l'afficher où bon vous semble. Il est bien évidemment possible de l'utiliser pour d'autres fichiers comme des pdf ou autres.

==> en dev == stockage en local

==> en prod == stockage dans des services comme Amazon S3, Google Cloud, ou Microsoft Azure

## Prérequi

### appli rails
### un Model + BDD
### un Controller + View associée

## $ rails active_storage:install

==> crée une migration active_storage_blobs == contient toutes les métadonnées des fichiers uploadés (taille, nom, type, etc.)

==> crée une migration active_storage_attachments == une table jointe entre tes modèles et les uploads

## $ rails db:migrate

## Lier des fichiers à des entrées en BDD (ex pour les users)

==> La première étape est d'indiquer au Model User qu'il pourra être lié à un objet d'Active Storage portant le nom avatar

		class User < ApplicationRecord
		  has_one_attached :avatar == avatar unique
	OU  has_many_attached :avatar == plusieurs avatars
		end

==> à partir de là, plusieurs méthodes d'instance user sont possibles

	user.avatar.attached? == si l'instance a un avatar(boolean)

	<%= form.file_field :avatar %> == permet l'upload du fichier de l'avatar dans la view

	user.avatar.attach(params[:avatar]) == lie l'avatar au user, une fois le fichier uploadé 

## Un controller avatars

Les avatars peuvent être considérés comme une ressource à part entière.

==> On va donc créer un avatars_controller, qui va héberger une def create et/ou new(si on passe par une page spécial d'upload de fichier)

	$ rails g controller avatars new create

==> dans routes.rb, on va imbriquer la route de l'avatar à celle du user

	Rails.application.routes.draw do
	  resources :users, only: [:show] do
	    resources :avatars, only: [:create]
	  end
	  # de cette manière un avatar est associé au user correspondant
	end

==> $ rails routes

	 Prefix Verb      URI          Pattern                            Controller#Action

	 user_avatars     POST  /users/:user_id/avatars(.:format)          avatars#create
	 new_user_avatar  GET   /users/:user_id/avatars/new(.:format)       avatars#new
	         user     GET  /users/:id(.:format)                          users#show

==> retour au avatars_controller

	class AvatarsController < ApplicationController

		def new
			@user = User.find(params[:user_id])
		end
	  def create

	    @user = User.find(params[:user_id])
	    #identifie l'utilisateur concerné

	    @user.avatar.attach(params[:avatar])
	    # on lui attribue l'avatar dont la référence est contenue dans params[:avatar]

	    redirect_to(user_path(@user))
	    # on redirige vers la page show de cet utilisateur

	  end
	end


##  Mise en place de les Views

==> l'upload de fichier

	<h3>Changer d'avatar ?</h3>
	<%= form_tag user_avatars_path(@user), multipart: true do %>
	  <%= file_field_tag :avatar %>
	  <%= submit_tag "mettre à jour" %>
	<% end %>

==> dans le show.html.erb du user: à insérer dans le code du show

	<h1>Page Profil</h1>

	<p>Bienvenue sur la page d'ajout d'un avatar pour le User portant l'id : <%=@user.id%></p>
	<h3>Avatar actuel</h3>
	<%if @user.avatar.attached?%>
	  <%= image_tag @user.avatar, alt: 'avatar' %>
	<%else%>
	  <p>=== Il n'y a pas encore d'avatar lié à cet utilisateur ===</p>
	  # on peut aussi imposer une photo de profil
	<%end%>


## Configurer Active Storage en production

==> stockage en local, dans config/environments/development.rb

	config.active_storage.service = :local (déjà présent)
	# les fichiers sont stockés dans le disque dur

==> en prod

De nombreux services refuseront de faire fonctionner Active Storage de cette façon, car ils ne veulent pas se retrouver à stocker des fichiers lourds.

Pour stocker les fichiers de ton app en production de façon pérenne, tu vas devoir passer par un hébergeur. Rails en recommande 3 : Amazon Web Services (AWS), Google Cloud Storage et Microsoft Azure. Il va donc te falloir : 

- Ouvrir un compte chez un de ces hébergeurs. Attention : ils vont tous te demander ta carte bancaire à l'inscription (même s'ils ont des offres gratuites);

- Récupérer tes clefs d'API pour connecter Active Storage chez eux;

- Mettre tes clefs dans ton app de façon sécurisée;

- Configurer Active Storage pour qu'il envoie les fichiers à ton hébergeur.

# Attention, cette fois-ci tu n'as plus le droit à l'erreur : si jamais tu push tes clefs d'API sur GitHub, vu que tu auras donné ta carte bleue au fournisseur, les conséquences pourraient être désastreuses pour ton compte en banque ! Si jamais ça t'arrivait, fonce sur le site de l'hébergeur (Amazon / Google / Microsoft) et révoque les clefs d'API qui ont été compromises. Ainsi elles cesseront de fonctionner et aucun petit malin ne pourra te ruiner en les utilisant.

#Mais le mieux reste quand même de bien te concentrer et de ne pas te rater dans l'utilisation de Dotenv et ton .gitignore.

==> Exemple avec Amazon

### Inscription + Récupérer des clefs d'API

### Configurer Active Storage

- dans config/environments/production.rb, change la ligne

		config.active_storage.service = :amazon

- dans storage.yml, décommenter les config de base du prestataire et met les clés API

		amazon:
			service: S3
			access_key_id: <%= ENV['AMAZON_ACCESS_KEY_ID'] %>
			secret_access_key: <%= ENV['AMAZON_SECRET_ACCESS_KEY'] %>
			region: us-east-1 
			#a priori, si tu es en France, il faut mettre ici : eu-west-3
			bucket: your_own_bucket
			#Mets ici le nom de ton bucket. Par exemple : prisme


- mettre les clés API dans .env et .env dans .gitignore

		AMAZON_ACCESS_KEY_ID= 'AKIAKOB5SEYHW8APSIYQ'
		AMAZON_SECRET_ACCESS_KEY= 'vCxbJWzolEJCLqZ4zXcSBgT5i9mAQCYMSw1zXyu'

- passer les clés API à Heroku

		$ heroku config:set AMAZON_ACCESS_KEY_ID=AKIAKOB5SEYHW8APSIYQ