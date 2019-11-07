
## pour un lien texte 

	<%= link_to "texte", ton_path %>

## pour un lien image dans app/assets/images

	<%= link_to (image_tag 'nom_image'), ton_path %>

## pour lien de différentes photos (active storage) vers son show 

		<% @projects.each do |project| %>
	    <div class="col-md-4 col-xs-12 portfolio_item">
	    <% if project.profil.attached? %> 
	      <%= link_to project_path(project) do%>
	      <%= image_tag project.profil, class: "img-responsive", alt: "image" %>
	      <div class="portfolio_item_hover">
	        <div class="portfolio-border">
	          <div class="item_info">
	            <span><%= project.title %></span>
	          </div>
	        </div>
	      </div>
	      <% end %>
	    <% end %>
	  <% end %>


## link_to avec un <span>

	<%= link_to "<span>L</span>og-out".html_safe, ton_path, ta_class: 'class_name' %>