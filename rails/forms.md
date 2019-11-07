https://m.patrikonrails.com/rails-5-1s-form-with-vs-old-form-helpers-3a5f72a8c78a

https://api.rubyonrails.org/

## form_tag

# method par default == post

	  <%= form_tag(ton_path, method: :get, post, put, past, delete) do %>
	    <%= label_tag(:attribut, "Search for:") %>
	    <%= text_field_tag(:attribut) %>
	    <%= submit_tag("Search") %>
	  <% end %>

Possibilités des champs

- checkbox

		<%= check_box_tag(:name, value='1', checked=true/false, options={}, disabled= true/false) %>

- color_field
		
		<%= color_field_tag(:name, value='color_code', options={}) %>

- date

		<%= date_field(:user, :born_on) %>

- email 

		<%= email_field_tag(:name, value=, options={})%>

- upload file
		
		<%= form_tag '/upload', multipart: true do %>
		  <%= file_field_tag "file" %>
		  <%= submit_tag %>
		<% end %>

option :disabled, :multiple, :accept

- hidden

		<%= hidden_field_tag(:parent_id, "5") %>

- image_submit_tag

		<%= image_submit_tag(source, options={})%> 

option :data, :disabled, confirm: 'question?'

- label

		<%= label_tag(:name) %>

- number

		<%= number_field_tag(:name, options={})%>

option min:, max:, in:, step:

- password

		<%= password_field_tag(:name, value=nil, options={})%> 

option diabled:, size:, maxlength:

- radio

		<%= radio_button_tag(:name, value, checked, options={})%> 

options disabled:

- range 

		<%= range_field_tag(:name, value, options)%>

options idem number

- seach

		<%= search_field_tag(name, value, options)%>

options same as text_field_tag

- select == dropdown selection box

		<%= select_tag(name, option_tags=nil, options)%> 

options multiple:true/false (multiple choices)
				disabled:true/false
				include_blank:true/false(option)
				prompt: == Create a prompt option with blank value and the text asking user to select something.

- submit

		<%= submit_tag(value='Save changes', oprions)%>

options :data == add custom data
					:data => {confirm: 'question'}
					:data => {disabled_with:true/false}
				disabled:true/false

- phone

		<%= telephone_field_tag(name, value, options)%> 

options: same as text_field_tag

- textarea

		<%= text_area_tag(name, content=nil, options)%>

options :size
				:rows == Specify the number of rows in the textarea 
				:cols == Specify the number of columns in the textarea
				:diabled
				:escape

- text_field

		<%= text_field_tag(name, value, :options => {})%>

options :disabled
				:size
				:maxlength
				:placeholder
				:value

- time 

		<%= time_field_tag(name, value, options)%>

options :min
				:max
				:step

- url

		<%= url_field_tag(name, value, options)%>

options same as text_field_tag


## form_for

# Permet de spécifier le Model !!!!

# Rails check s'il est de type create (donc si l'objet a été créé), edit

	  <%= form_for @article do |f| %>
	    <%= f.text_field :title %>
	    <%= f.text_area :body, size: "60x12" %>
	    <%= f.submit "Create" %>
	  <% end %>


# Possibiltés de champs == idem que form_tag, mais sans le tag et avec f. !!!!

Pour les partials: 

	<%= form_for(resource, url: path, method: method)  do |f| %>
	<% end %>


## form_with

# Fonctionne directement avec JS et AJAX !