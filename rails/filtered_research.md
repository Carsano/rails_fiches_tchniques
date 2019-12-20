
# Recherche filtrée

- recherche html et pas js
https://stackoverflow.com/questions/19760527/rails-filter-index-results-by-using-a-link-without-dropdown

- recherche js + cases à cocher
https://medium.com/swlh/advanced-filtering-for-your-rails-5-application-28c8da2d29b6

- gem 'pg_search'
https://github.com/Casecommons/pg_search

- autres manière de filtrer (non testé)
https://www.justinweiss.com/articles/search-and-filter-rails-models-without-bloating-your-controller/

ex: 
- 1 model Project avec deux attributs category et content, et branché avec activestorage
- has_many_attached:pictures
- Project créé dans une interface admin
- root to: "Projects#index"
- présentation de toutes les photos
- menu présentant les différentes catégories
- dès qu'on clique sur une catégory ==> présentations des photos de la catégory en AJAX

## prérequis

gem 'jquery-rails'

gem "pg_seach" dans Gemfile

$ bundle install


## project.rb

==> recherche à partir de l'attribut 'category'

	class Project < ApplicationRecord
		has_many_attached :pictures

		include PgSearch::Model
		# branche le model à la gem 

		pg_search_scope :search_by_category,
										# n'importe quel nom
		 against: :category
		 # concerne l'attribut category
	end


## views/projects/index.html.erb

==> pour le menu de sélection cliquable

	<%= link_to "Toutes les photos", projects_path, class: "cbp-filter-item", remote: true %>
	# séparation de toutes les photos avec les sélections

  <% @projects.each do |project| %>
    <%= link_to project.category, projects_path(category: project.category), class: "cbp-filter-item", remote: true %>
  <% end %>
  # concerne les liens de sélections
  # remote: true pour l'ajax

==> pour le display des photos, mis dans une partial, qui servira aussi pour le fichier ajax

	<div id="projects" class="card-columns">
	  <%= render partial: 'projects_display' %>
	</div>

id est important pour l'ajax

## views/projects/_projects_display.html.erb

	<% @projects.each do |project| %> 
	    <% if project.pictures.attached?%>
	      <% project.pictures.each do |picture| %>
	        <div class="interior">
	        <figure class="overlay overlay2">
	          <%= link_to url_for(picture) do%>
	            <span class="bg"></span>
	            <%= image_tag(picture) %>
	          <% end %>
	        </figure>
	        </div>
	      <% end %>
	    <% end %>
	<% end %>


## projects_controller.rb

==> concerne la méthode index 

	class ProjectsController < ApplicationController
	  def index
	  	if params[:category]
	  	# si le params renvoi une category
	  		@filter = params[:category]
		    @projects = Project.all.search_by_category("#{@filter}")
		    # search_by_categories == ce qu'on a mis dans le model
		  else
		  # sinon
		    @projects = Project.all
		  end

		  respond_to do |format|
			  format.html
			  format.js
			end
			# pour l'ajax
	  end
	end


## Créer index.js.erb

	function replaceProjects (innerHTML) {
	  const projects = document.getElementById('projects');
	  # on retrouve l'id 
	  projects.innerHTML = innerHTML;
	}

	replaceProjects("<%= j render partial: 'projects_display' %>");


## And it's done !
