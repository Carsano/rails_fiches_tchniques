
# ----------------- INTERFACE ADMIN -----------------

https://www.grafikart.fr/tutoriels/tp-admin-858

## Créer une migration pour ajouter une colonne admin(si pas fait lors de la création du Model User)

==> true == le user est administrateur de l'appli

==> false == le user n'est pas admin

### $ rails g migration RoleToUsers

### add_column :users, :admin, :boolean, :default => false (de base, les users ne sont pas admin)

### $rails de:migrate

## insertion de cette nouvelle condition dans la barre de nav

==> si le current_user == user connecté(vérifié par la condition user_signed_in?) est admin ou pas 

==> dans layouts/application.html.erb, par ex:

		<div class="dropdown-menu">

	    <% if current_user.admin == true %>
	      <p><%= link_to "Espace admin", admin_users_path %></p>
	    <% end %>
	    # affichage de l'espace admin que si current_user est admin
	      <p><%= link_to "Ton profil", user_path(current_user.id) %></p>
	      <p> <%= link_to "Deconnecte-toi", destroy_user_session_path, method: :delete %></p>
	  </div>

==> on peut créer une méthode check_if_admin?

	def check_if_admin?
		user_signed_in? && current_user.admin == true
	end

## dans app/controllers

### Créer un dossier admin qui va contenir les controllers relatif à la section admin

==> avantage de différencier les controllers de la section admin des autres controllers de l'appli

### Dans le dossier admin

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


## Dans les routes: routes.rb

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


## liens routes vers la page admin dans layouts/application.html.erb

### <%= link_to "Espace admin", prefix_routes_path %>

## Les Views

==> créer un dossier à part admin

==> dedans, créer un sous-dossier pour chaque controller 

==> créer un index.html.erb ou plus pour chaque controller

## Restriction accès page admin

#Si on tape l'url de l'espace admin, tout le monde y a accès!!!

==> dire qu'on veut limiter cette url au admin

==> dans application.html.erb de admin, rajouter:

	module Admin

		class ApplicationController < ::ApplicationController

			before_filter :only_admin

			private
			
			def only_admin
				if user_signed_in? && current_user.admin == false
					flash[:errors] = "Vous n'avez pas le droit d'accéeder à cette page!"	
					redirect_to events_path
				elsif user_signed_in? == false
					flash[:errors] = "Vous n'avez pas le droit d'accéeder à cette page!"
					redirect_to events_path
				end
			end

		end

	end

==> cas d'un unique accès admin user == admin (création en bd)

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











