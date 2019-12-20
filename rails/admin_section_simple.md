## Avec gem 'bcrypt'

==> cf gem_bcrypt.md

# Une fois un user crée en console

# Redémarrer le serveur!!!

## Session

En Rails, session est un hash qui est stocké dans un coin du navigateur web et qui va suivre le visiteur tout au long de sa navigation. 
Il est grosso modo créé quand ce dernier arrive sur ton site, et est détruit quand il ferme son navigateur. 
À l'inverse de params, qui se vide de son contenu à chaque changement de page, le hash session conserve les informations jusqu'à fermeture du navigateur. 

Ainsi, à chaque nouvelle page chargée, on pourra retrouver quel utilisateur est en train de la visualiser. 
Tu feras ça dans tes controllers avec les lignes suivantes:

	id = session[:user_id]
	@user = User.find(id) #et hop, cette variable @user est l'instance User contenant toutes les infos de l'utilisateur connecté

# Pour gérer les connexions à ton app Rails, on va gérer des "sessions" via un sessions_controller qui contiendra les méthodes avec #new, #create, et #destroy. 

- new == login

- create == authentification 
Si ce dernier est bien authentifié, nous stockerons l'info avec session[:user_id] = user.id. S'il n'est pas bien authentifié, on fera un render de la page de login avec les messages d'erreur.

# Une fois l'utilisateur connecté, ce sera très simple de retrouver notre utilisateur grâce à User.find(session[:user_id])

- destroy == déconnexion

## sessions_controller

### $ rails g controller sessions new create destroy

Une session est créée à l'instant où un utilisateur se connecte. 
On a alors une information dans le hash session[:user_id]. 
Si personne n'est connecté => session[:user_id] = nil.

==> new + create

	class SessionsController

		def new
			#sert uniquement à avoir une view pour demander le nom et mot de passe
		end

		def create
		  # cherche s'il existe un utilisateur en base avec l’e-mail
		  user = User.find_by(email:params[:email])
		  ou nom
		  user = User.find_by(name:params[:name])

		  # on vérifie si l'utilisateur existe bien ET si on arrive à l'authentifier (méthode bcrypt) avec le mot de passe 
		  if user && user.authenticate(params[:password])
		    session[:user_id] = user.id
		    # redirige où tu veux, avec un flash ou pas

		  else
		    flash.now[:danger] = 'Invalid email/password combination'
		    render 'new'
		  end
		end

	end

==> destroy

Destroy permet de déconnecter l'utilisateur. Cliquer sur le bouton "Se déconnecter" revient à supprimer le contenu de session[:user_id].
Le lien de ton bouton "Se déconnecter" devrait ressembler à ceci:

	<%= link_to "Se déconnecter", session_path(session.id), method: :delete %>

Dans sessions_controller

	def destroy
	  session.delete(:user_id)
  	session[:user_id] = nil
	end

### Routes.rb

resources :sessions 

## Helpers

==> intro

Nous allons apprendre à changer un User.find_by(id: session[:user_id]) en un plus simple current_user avec deux raisons en tête :

    En Rails, c'est le mal de faire un appel au model (= une requête SQL) dans une vue. C'est le rôle du controller ! Le moindre find dans la view et c'est -30 points en test technique.
    En Rails on aime le code bien DRY et bien lisible. current_user c'est très lisible et plus court que l'autre pâté.

Pour refactorer (= condenser) de cette façon notre code, nous allons passer par un helper. Les helpers se rangent dans le dossier app/helpers/ : ils ont pour mission de créer des méthodes pour remplacer les bouts de code qu'on utilise fréquemment. Tout ce que tu as à faire, c'est de mettre dans ton controller : 

	class TonController < ApplicationController
	  include TonHelper
	end

TonHelper correspond au fichier app/helpers/ton_helper.rb qui devrait ressembler à ceci :

	module TonHelper
	  def some_method
	    # une méthode et son code
	  end
	end

Nous allons donc créer un helper pour les sessions des utilisateurs. 

#Toutefois, il faut savoir qu'on va y faire appel dans quasiment TOUS les controllers et se taper un include SessionsHelper dans chacun d'eux, ça va vite être fastidieux. La solution, c'est de rajouter la ligne dans app/controllers/application_controller.rb qui est, en quelque sorte, le parent de tous les controllers.

Donc une fois la ligne rajoutée, il faut créer un fichier app/helpers/sessions_helper.rb et y mettre les lignes suivantes :

	module SessionsHelper
	   def current_user
		   if session[:user_id]
		     @current_user = User.find_by(id: session[:user_id])
		   end
		 end

	  def log_in(user)
	    session[:user_id] = user.id
	  end

	end

## sessions Views

Dans sessions#new (formulaire type)

	<div class="container form-center">
	  <h2>Identification</h2>
	  <br>
	  <%= form_tag url_for(action: 'create'), method: 'post' do %>
	   <fieldset>
	     <div>
	       <%= label_tag :name, 'Nom' %><br>
	       <%= text_field_tag :name, params[:name] %>
	     </div><br>
	     <div>
	       <%= label_tag :password, 'Mot de passe' %><br>
	       <%= password_field_tag :password, params[:password] %>
	     </div>
	     <div>
	      <br>
	       <%= submit_tag 'Login' %>
	     </div>
	   </fieldset>
	  <% end %>
	</div>

# Test affichage

## Views pour navbar (liens espace admin + log_out)

		<%if current_user != nil %>
		  <%= link_to "Log-out", session_path(:id), :method => :delete, data: {confirm: "veux tu vraiment de déconnecter ?"} %>
		  <li><%= link_to 'Espace admin', ton_path %>
		<% end %>

# Tests validation connexion/deconnexion

## Creation espace admin

### dans app/controllers

#### Créer un dossier admin qui va contenir les controllers relatif à la section admin

==> avantage de différencier les controllers de la section admin des autres controllers de l'appli

#### Dans le dossier admin

==> créer un controller application_controller.rb 

==> dedans

	module Admin

		class ApplicationController < ::ApplicationController
					# extend de l'ApplicationController de la racine 

			# permet notamment d'utiliser un autre layout que celui de application.html.erb racine
			ex: layout 'admin'

		end

	end

==> créer les autres controllers admin dont on a besoin

Dans Eventbrite, l'admin a besoin de gérer les users, les events, les attendees

	=> users_controller.rb 
	=> events_controller.rb 
	=> attendees_controller.rb 

==> dans chacun(ex pour users_controller.rb)

	module Admin

		class UsersController < ApplicationController
			# UsersController dépend de ApplicationController du dossier admin qui dépend du ApplicationController racine

			def index
				# par ex: @user = User.all 
			end 

			def create
			end

			etc

		end

	end


### Dans les routes: routes.rb

==> utilisation du namespace

	Rails.application.routes.draw do

	  namespace :admin do
	  resources :users, :events
	  end

	end

==> $ rails routes == création de nouvelles routes

             admin_users GET     /admin/users(.:format)                                                                   admin/users#index
                          POST   /admin/users(.:format)                                                                   admin/users#create
           new_admin_user GET    /admin/users/new(.:format)                                                               admin/users#new
          edit_admin_user GET    /admin/users/:id/edit(.:format)                                                          admin/users#edit
               admin_user GET    /admin/users/:id(.:format)                                                               admin/users#show
                          PATCH  /admin/users/:id(.:format)                                                               admin/users#update
                          PUT    /admin/users/:id(.:format)                                                               admin/users#update
                          DELETE /admin/users/:id(.:format)                                                               admin/users#destroy
             admin_events GET    /admin/events(.:format)                                                                  admin/events#index
                          POST   /admin/events(.:format)                                                                  admin/events#create
          new_admin_event GET    /admin/events/new(.:format)                                                              admin/events#new
         edit_admin_event GET    /admin/events/:id/edit(.:format)                                                         admin/events#edit
              admin_event GET    /admin/events/:id(.:format)                                                              admin/events#show
                          PATCH  /admin/events/:id(.:format)                                                              admin/events#update
                          PUT    /admin/events/:id(.:format)                                                              admin/events#update
                          DELETE /admin/events/:id(.:format)                                                              admin/events#destroy


### liens routes vers la page admin dans layouts/application.html.erb

	 <%= link_to "Espace admin", prefix_routes_path %>

### Les Views

==> créer un dossier à part admin

==> dedans, créer un sous-dossier pour chaque controller 

==> créer un index.html.erb ou plus pour chaque controller

### Restriction accès page admin

#Si on tape l'url de l'espace admin, tout le monde y a accès!!!

==> dire qu'on veut limiter cette url au admin

	module Admin
		class ApplicationController < ::ApplicationController
			before_action :only_admin

				private
				
				def only_admin
					if current_user == nil
						flash[:errors] = "Vous n'avez pas le droit d'accéeder à cette page!"	
						redirect_to root_path
					end
				end
		end
	end


### before_action

Prenons l'exemple d'une page qui ne peut être accessible QUE par un utilisateur connecté (par exemple une page en /dashboard). Comment mettre en place un filtre pour rejeter les personnes non connectées ? Ça c'est une mission pour le controller ! Eh bien c'est exactement le taf du controller ! Ce dernier va vérifier si l'utilisateur est login, et puis il va le rediriger vers la page de login s'il n'est pas connecté.

On pourrait imaginer de faire quelque chose comme ça dans la méthode qui pointe vers /dashboard:

	def index
	unless current_user
	  flash[:danger] = "Please log in."
	  redirect_to new_session_path
	else
	  # on code quelque chose qui permet d'afficher le dashboard de l'utilisateur
	end

Sauf qu'on peut faire ça beaucoup plus proprement. La bonne façon de le présenter, c'est de faire ça dans une méthode privée authenticate_user, et de l'appeler EN AMONT de la méthode du controller, grâce à un callback before_action. Voici le résultat:

	class TonController < ApplicationController
	  before_action :authenticate_user, only: [:index]

	  def index
	    # on code quelque chose qui permet d'afficher le dashboard de l'utilisateur
	  end

	  private

	  def authenticate_user
	    unless current_user
	      flash[:danger] = "Please log in."
	      redirect_to new_session_path
	    end
	  end

	end

