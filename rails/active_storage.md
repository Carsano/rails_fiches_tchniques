
# -------------- Active Storage -----------------

https://www.engineyard.com/blog/active-storage

https://edgeguides.rubyonrails.org/active_storage_overview.html

https://prograils.com/posts/rails-5-2-active-storage-new-approach-to-file-uploads

### pour has_many_attached chargements de plusieures photos en même temps
https://www.youtube.com/watch?v=A23zCePXe74
https://evilmartians.com/chronicles/rails-5-2-active-storage-and-beyond

Active Storage est une fonctionnalité sympathique de Rails qui permet à un utilisateur de votre app de mettre en ligne des fichiers qui pourront ensuite être utilisés dans son site web. L'exemple typique est le fait d'envoyer une photo de profil : celle-ci est alors stockée dans le cloud et l'app Rails peut l'appeler à tout moment afin de l'afficher où bon vous semble. Il est bien évidemment possible de l'utiliser pour d'autres fichiers comme des pdf ou autres.

==> en dev == stockage en local

==> en prod == stockage dans des services comme Amazon S3, Google Cloud, ou Microsoft Azure

## Prérequi

### appli rails

### un Model + BDD

$ rails g model ModelName ....

$ rails db:migrate

### un Controller + View associée

$ rails g controller index ....

## $ rails active_storage:install

==> crée une migration active_storage_blobs == contient toutes les métadonnées des fichiers uploadés (taille, nom, type, etc.)

==> crée une migration active_storage_attachments == une table jointe entre tes modèles et les uploads

## $ rails db:migrate



## has_one_attached

## Lier des fichiers à des entrées en BDD (ex pour les users)

==> La première étape est d'indiquer au Model User qu'il pourra être lié à un objet d'Active Storage portant le nom avatar

		class User < ApplicationRecord
		  has_one_attached :avatar == avatar unique
		end

==> à partir de là, plusieurs méthodes d'instance user sont possibles

	user.avatar.attached? == si l'instance a un avatar(boolean)

	<%= form.file_field :avatar %> == permet l'upload du fichier de l'avatar dans la view

	user.avatar.attach(params[:avatar]) == lie l'avatar au user, une fois le fichier uploadé 


## Un controller avatars

Les avatars peuvent être considérés comme une ressource à part entière.

==> On va donc créer un avatars_controller, qui va héberger une def create et/ou new(si on passe par une page spécial d'upload de fichier)

	$ rails g controller avatars new create destroy

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
		
		def show
			@user = Project.find(params[:user_id])
		end

	  def create

	    @user = User.find(params[:user_id])
	    #identifie l'utilisateur concerné

	    @user.avatar.attach(params[:avatar])
	    # on lui attribue l'avatar dont la référence est contenue dans params[:avatar]

	    redirect_to(user_path(@user))
	    # on redirige vers la page show de cet utilisateur

	  end

		def destroy
			@user = Project.find(params[:user_id])
		  @user.avatar.purge
		  redirect_to edit_admin_project_path(@project)
		end
	end

# Si interface admin_session_simple

==> créer le controller à la main dans Admin

	module Admin
		class AvatarsController < ApplicationController
			def show
				@project = Project.find(params[:project_id])
			end

			def create
				@project = Project.find(params[:project_id])

				@project.avatar.attach(params[:avatar])
		    redirect_to ton_path
			end

			def destroy
				@project = Project.find(params[:project_id])
			  @project.avatar.purge
			  redirect_to ton_path
			end
		end
	end

==> dans routes.rb, on va imbriquer la route de l'avatar à celle de l'admin

	Rails.application.routes.draw do
	  namespace :admin do
		  resources :projects do
		  	resources :avatars
		  end
		end
	  # de cette manière un avatar est associé au projet correspondant de la section admin
	end


##  Mise en place de les Views

==> l'upload de fichier

	<h3>Changer d'avatar ?</h3>
	<%= form_tag user_avatars_path(@user), multipart: true do %>
	  <%= file_field_tag :avatar %>
	  <%= submit_tag "mettre à jour" %>
	<% end %>

==> dans le show.html.erb du user

	<h1>Page Profil</h1>

	<p>Bienvenue sur la page d'ajout d'un avatar pour le User portant l'id : <%=@user.id%></p>
	<h3>Avatar actuel</h3>
	<%if @user.avatar.attached?%>
	  <%= image_tag @user.avatar, alt: 'avatar' %>
	<%else%>
	  <p>=== Il n'y a pas encore d'avatar lié à cet utilisateur ===</p>
	  # on peut aussi imposer une photo de profil
	<%end%>

==> bon à savoir, téléchargement de l'avatar 

	<%= link_to 'Download', @user.avatar, download: ''%>


# Si admin_session_simple

==> mettre les views dans le dossier admin



## has_many_attached

### model 

		class User < ApplicationRecord
			has_many_attached :avatars 
		end

==> à partir de là, plusieurs méthodes d'instance user sont possibles

	user.avatars.attached? == si l'instance a un avatar(boolean)

	<%= form.file_field :avatars %> == permet l'upload du fichier de l'avatar dans la view

	user.avatars.attach(params[:avatars]) == lie l'avatar au user, une fois le fichier uploadé 

### Tests

==> $ rc et création d'un objet

user = User.create(....)

==> dans le controller, def show 

@user = User.find(params[:id]) 

==> dans routes.rb 

resources :users, only: [:show]

==> dans show.html.erb 

	<h1>Page Profil</h1>

	<p>Bienvenue sur la page d'ajout d'un avatar pour le User portant l'id : <%=@user.id%></p>
	<h3>Avatar actuel</h3>
	<%if @user.avatars.attached?%>
	  <% @user.avatars.each do |image| %>
	  	<%= image_tag image %> <br />
		<% end %>
	<%else%>
	  <p>=== Il n'y a pas encore d'avatar lié à cet utilisateur ===</p>
	<%end%>

==> test en faisant .../users/1

### Un controller avatars

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

	  def create

	    @user = User.find(params[:user_id])
	    #identifie l'utilisateur concerné

	    @user.avatar.attach(params[:avatar])
	    # on lui attribue l'avatar dont la référence est contenue dans params[:avatar]

	 OU @user.avatars.attach(params[:avatars]) 
	 		# si has_many_attached

	    redirect_to(user_path(@user))
	    # on redirige vers la page show de cet utilisateur

	  end
	end

==> Si interface admin, dans admin/files_controller.rb 

	module Admin

		class FilesController < ApplicationController

			def show
				@file = ActiveStorage::Attachment.find(params[:id])
				# permet d'afficher la file 

			end

			def create
				@project = Project.find(params[:project_id])

				if @project.files.attach(params[:files])
		    	flash[:success] = 'Photos chargées!'
		    	redirect_to admin_project_path(@project)
		   #  else
		   #  	flash[:error] = 'Ya un blème... Retentes!'
					# render 'new'
				end
			end

			def edit
				
			end

			def update
				
			end

			def destroy
				
			end

			private

			def files_params

			end
		end

	end

##  Mise en place de les Views

==> l'upload de fichier 

	<h3>Changer d'avatar ?</h3>

	<%= form_tag project_files_path(@project), multipart: true do |file| %>
	  <%= file_field_tag :files, multiple: true %>
	  													# permet de charger plusieurs files en une fois
	  													# ou multiply si chargements en plusieurs fois
	  <%= submit_tag "mettre à jour" %>
	<% end %>

==> dans le show.html.erb du user: à insérer dans le code du show 

	<h1>Page Profil</h1>

	<p>Bienvenue sur la page d'ajout d'un avatar pour le User portant l'id : <%=@user.id%></p>
	<h3>Avatar actuel</h3>
	<%if @user.avatars.attached?%>
	  <% @user.avatars.each do |image| %>
	  	<%= image_tag image %> <br />
		<% end %>
	<%else%>
	  <p>=== Il n'y a pas encore d'avatar lié à cet utilisateur ===</p>
	<%end%>

## Accéder aux data de l'image

	ActiveStorage::Analyzer::ImageAnalyzer.new(self.profil).metadata

==> sort {:width => '', :height => ''}

==> .metadata[:width]

==> .metadata[:height]

## On peut combiner avec gem 'mini_magick'

cf ruby/gems/gem_mini_magick.md


# Pour has_many_attached
# @user.avatars == array d'avatars!!!!
# donc pour afficher le premier == @user.avatars[0] ......

# Avec cette méthode pour has_many_attached == chargement de plusieures photos, mais une à la fois!!!!
# Sinon

https://www.youtube.com/watch?v=A23zCePXe74



## Configurer Active Storage en production

# Supprimer les attachment active_storage lors du passage à heroku

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

https://medium.com/alturasoluciones/setting-up-rails-5-active-storage-with-amazon-s3-3d158cf021ff

https://www.youtube.com/watch?v=RKJQOj6RG9g (début)

==> créer son compte aws

==> rentrer cb

==> taper S3 dans la barre de recherche AWS services et sélectionner S3 scalable storage in the cloud

==> cliquer sur Create bucket

- Name and region: remplir bucket name + region
- Configure options: next
- Set permissions: next 
- Review: create bucket

==> clique sur Services

==> taper IAM dans la barre de recherche

==> sélectionner IAM Manage user access and exceptions keys

==> cliquer sur users 

==> add user

- rentrer un nom de user + access type 'Programmatic access'
- next:permissions
- cliquer sur Attach existing policies
- cocher AmazonS3FullAccess 
- next:tags
- next:review
- next:create

==> clés API du users

==> laisser la fenêtre ouverte

### Configurer Active Storage

- dans storage.yml, décommenter les config de base du prestataire

		amazon:
			service: S3
			access_key_id: <%= Rails.application.credentials.dig(:aws, :access_key_id) %>
			secret_access_key: <%= Rails.application.credentials.dig(:aws, :secret_access_key) %>
			region: us-east-1
			bucket: your_own_bucket

 - dans Gemfile

		gem "aws-sdk-s3", require: false

$ bundle install

 - dans storage.yml, mettre les clés API

		amazon:
			service: S3
			#access_key_id: <%= ENV['AMAZON_ACCESS_KEY_ID'] %>
			#secret_access_key: <%= ENV['AMAZON_SECRET_ACCESS_KEY'] %>
			region: us-east-1 
			#a priori, si tu es en France, il faut mettre ici : eu-west-3
			bucket: your_own_bucket
			#Mets ici le nom de ton bucket


- créer fichier .env, mettre .env dans .gitignore et copier/mettre les clés API du user créé sur AWS

		AMAZON_ACCESS_KEY_ID= 'AKIAKOB5SEYHW8APSIYQ'
		AMAZON_SECRET_ACCESS_KEY= 'vCxbJWzolEJCLqZ4zXcSBgT5i9mAQCYMSw1zXyu'


- dans config/environments/development.rb, change la ligne

		config.active_storage.service = :amazon

- aller voir dans le bucket créé si l'image chagée y est bien!!!

# Test en local!! N'a pas marché pour moi.... 

- dans config/environments/production.rb, change la ligne

		config.active_storage.service = :amazon


### passer les clés API à Heroku

https://devcenter.heroku.com/articles/config-vars

		$ heroku config:set AMAZON_ACCESS_KEY_ID=AKIAKOB5SEYHW8APSIYQ

OU plus simple

==> les rentrer dans config var sur heroku!!

==> les mettres en staging et en production

### $ git add + git push origin master

### $ git push heroku master

### $ heroku run bundle install

