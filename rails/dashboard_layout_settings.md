#***********  Organisation d'un dashboard admin ou user *************
#*************  Appliquer un layout particulier *******************


# FAIRE UN SERVICE QUI GERE LES LAYOUTS EN FONCTION DU CONTROLLER ET DE L’ACTION

## Prérequis

==> Système de login/logout qui fait accéder à un dashboard(devise ou bcrypt)

==> Créer dashboard.html.erb dans views/layout == template pour le dashboard

ex: 

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>EspaceAdmin</title>
	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
	    <%= csrf_meta_tags %>
	    <%= csp_meta_tag %>
	    <%= stylesheet_link_tag    'application', media: 'all' %>
	    <%= javascript_include_tag 'application' %>
	  </head>
	  <body id="page-top">
	     <%= render 'layouts/navbar'%>
	     # partial pour la navbar général
	     # on peut créer une partial pour une navbar spécifique au dashboard
	    <div id="wrapper">
	      <%= render 'layouts/user_sidenav'%>
	      # partial pour la sidebar de l'interface
	      <div id="content-wrapper">
	        <%= yield %>
	      
	      </div>
	    </div>
	 </body>
	</html>

==> reprendre le principe de création de dossier dans controllers et views de rails/admin_section.md

==> Dans routes.rb, spécifier les routes pour le dashboard

	namespace :dashboard do
	  resources :projects, :users
	end



## Dans app/controllers/application_controller.rb, préciser que le layout sera géré par une méthode spécifique qu’on appelle ici “layout_manager”

	class ApplicationController < ActionController::Base
		layout :layout_manager
	end


## Créer un dossier “utilities” dans le app/controllers

## Puis y créer un fichier qu’on appelle “layout_manager.rb” qui contiendra notre service. 

Pour l’instant, on peut juste écrire la structure du service : 
 
	module Utilities
	  class LayoutManager
	    class << self  	
	    ...
	  end 
	end


## Dans app/controllers/application_controller.rb, écrire la méthode “layout_manager” qui va juste appeler le service LayoutManager avec 2 paramètres en entrée : le controller_name et l’action_name


	class ApplicationController < ActionController::Base
		layout :layout_manager

		private
		
		def layout_manager
	    Utilities::LayoutManager.call(
	        controller_name: params[:controller],
	        action_name: params[:action]
	    )
	  end
	end


N.B. : Pour bien comprendre comment on peut récupérer facilement params[:controller] et params[:action] :
    • mettre un binding.pry dans une des méthodes d’un controller (ex : méthode index du controller article) puis lancer le serveur et tenter d’aller sur la view index d’articles
    • dans la console PRY, on tape params et ça affiche bien : 
    • => <ActionController::Parameters {"controller"=>"articles", "action"=>"index"} permitted: false>
    • on peut récupérer ces données et les mettre dans 2 variables “controller_name” et “action_name” !!

N.B.2 : on aurait très bien pu mettre .perform au lieu de .call. Mais .call est plus approprié ici car :
Tarek : “C'est juste qu'en ruby la méthode call et faite pour ce cas d'usage. Elle offre notamment la possibilité syntaxique suivante : MyService.(*args) (plutôt que MyService.call(*args)).
Et cela donne au lecteur du code l'indication qu'il s'agit d'un Function Object (qui n'a qu'une méthode publique)”


## Dans app/controllers/utilities/layout_manager.rb

==> il faut compléter pour faire afficher le bon layout en fonction du controller et de l’action.

==> d’abord il faut qu’on identifie dans des constantes (=array) :

• Les controllers pour lesquels on affiche le dashboard pour toutes les actions
• Les controllers pour lesquels on affiche le dashboard pour toutes les actions, sauf pour show 
• Les controllers devise pour lesquels on veut afficher le dashboard et non le application.html.erb qui se lance par défaut (ici c’est la view edit qu’on veut différente des autres)

Ça donne : 

		module Utilities

		  class LayoutManager

		    DISPLAY_DASHBOARD = %w[dashboard/users].freeze
		    DISPLAY_DASHBOARD_EXCEPT_SHOW = %w[dashboard/projects].freeze
		    DISPLAY_DASHBOARD_FOR_DEVISE = %w[devise/registrations].freeze
		    # adapter en fonction du nombre de controller dans l'espace admin

		    class << self
		    end
		  end
		end

NB : Nom en majuscule car ce sont des constantes.
NB : %w devant un array permet de ne pas écrire les “ “ autour de chaque mot
NB : le .freeze permet de n’apporter aucune modification ultérieure à ces constantes. 


==> on écrit le case when pour prévoir tous les cas de figure et renvoyer le bon layout en fonction du controller et de l’action ! En gros c’est >> when ‘controller x’ then ‘layout y’


	module Utilities

	  class LayoutManager

	    DISPLAY_DASHBOARD = %w[dashboard/users].freeze
	    DISPLAY_DASHBOARD_EXCEPT_SHOW = %w[dashboard/projects].freeze
	    DISPLAY_DASHBOARD_FOR_DEVISE = %w[devise/registrations].freeze

	    class << self

	      def call(controller_name:, action_name:)
	        case controller_name
	        when 'static_pages' then 'static_pages'
	        when *DISPLAY_DASHBOARD then 'dashboard'
	        when *DISPLAY_DASHBOARD_EXCEPT_SHOW then dashboard_except_show(action_name)
	        when *DISPLAY_DASHBOARD_FOR_DEVISE then devise_route_to_dashboard(action_name)
	        else
	          'application'
	        end
	      end
	    end
	  end
	end


NB : le * devant DISPLAY permet de remplacer un when ARRAY[0] || ARRAY[1] || ARRAY[2] ...etc → gain de place considérable !!!!


==> Enfin il faut juste écrire 2 petites méthodes privées pour gérer les exceptions d’action (show pour les controllers articles / events / projects et edit pour le controller devise)

	module Utilities

	  class LayoutManager

	    DISPLAY_DASHBOARD = %w[dashboard/users].freeze
	    DISPLAY_DASHBOARD_EXCEPT_SHOW = %w[dashboard/projects].freeze
	    # DISPLAY_DASHBOARD_FOR_DEVISE = %w[devise/registrations].freeze

	    class << self

	      def call(controller_name:, action_name:)
	        case controller_name
	        when 'static_pages' then 'static_pages'
	        when *DISPLAY_DASHBOARD then 'dashboard'
	        when *DISPLAY_DASHBOARD_EXCEPT_SHOW then dashboard_except_show(action_name)
	        when *DISPLAY_DASHBOARD_FOR_DEVISE then devise_route_to_dashboard(action_name)
	        else
	          'application'
	        end
	      end

	      private

	      def dashboard_except_show(action_name)
	        action_name == 'show' ? 'application' : 'dashboard'
	      end

	      def devise_route_to_dashboard(action_name)
	        action_name == 'edit' ? 'dashboard' : 'application'
	      end

	    end

	  end

	end

## CSS

ajouter le css pour la mise en page


## Exemple

Site avec connexion bcrypt 
Model User
Dashboard pour un photographe
Dashboard pour autres users

### Dashboard Photo

==> dashboard_photographer.html.erb dans views/layouts

	<!DOCTYPE html>
		<html>
		  <head>
		    <title>EspaceAdmin</title>
		    <meta name="viewport" content="width=device-width, initial-scale=1.0">
		    <%= csrf_meta_tags %>
		    <%= csp_meta_tag %>
		    <%= stylesheet_link_tag    'application', media: 'all' %>
		    <%= javascript_include_tag 'application' %>
		  </head>
		  <body id="page-top">
		     <%= render 'layouts/photographer_navbar'%>
		     # créer partial 
		    <div id="wrapper">
		      <%= render 'layouts/photographer_sidenav'%>
		      # créer la partial
		      <div id="content-wrapper">
		        <%= yield %>
		      
		      </div>
		    </div>
		 </body>
	</html>

==> dossier dashboard_photographer dans app/controllers avec les sous-dossiers qu'on veut

==> dossier dashboard_photographer dans app/views avec les sous-dossiers qu'on veut

==> dans routes.rb

	  namespace :dashboard_photographer do
		  resources :projects, :users
		end

==> Dans app/controllers/application_controller.rb, préciser que le layout sera géré par une méthode spécifique qu’on appelle ici “layout_manager”

	class ApplicationController < ActionController::Base
		layout :layout_manager

		private
		
		def layout_manager
	    Utilities::LayoutManager.call(
	        controller_name: params[:controller],
	        action_name: params[:action]
	    )
	  end
	end

==> Créer un dossier “utilities” dans le app/controllers

==> Dedans

	module Utilities
	  class LayoutManager

	    DISPLAY_DASHBOARD = %w[dashboard_photographer/users].freeze
	    DISPLAY_DASHBOARD_EXCEPT_SHOW = %w[dashboard_photographer/projects].freeze
	    # DISPLAY_DASHBOARD_FOR_DEVISE = %w[devise/registrations].freeze

	    class << self

	      def call(controller_name:, action_name:)
	        case controller_name
	        # when 'static_pages' then 'static_pages'
	        when *DISPLAY_DASHBOARD then 'dashboard_photographer'
	        when *DISPLAY_DASHBOARD_EXCEPT_SHOW then dashboard_photographer_except_show(action_name)
	        # when *DISPLAY_DASHBOARD_FOR_DEVISE then devise_route_to_dashboard(action_name)
	        else
	          'application'
	        end
	      end

	      private

	      def dashboard_photographer_except_show(action_name)
	        action_name == 'show' ? 'application' : 'dashboard_photographer'
	      end

	      # def devise_route_to_dashboard(action_name)
	      #   action_name == 'edit' ? 'dashboard' : 'application'
	      # end

	    end
	  end
	end


