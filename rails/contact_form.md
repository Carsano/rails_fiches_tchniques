https://gist.github.com/stevecondylios/e819a296167a31578d82fac881963789

https://www.youtube.com/watch?v=QIoORYeBdhs

In July 2019, Heroku updated the bundler requirement, so we simply need to upgrade bundler locally (or else we'll get an error message when pushing to Heroku)

	gem install bundler -v 2.0.2
	bundle update --bundler

## Gemfile

gem 'bootstrap-sass', '~> 3.3', '>= 3.3.6' (non installé, pb...)
gem 'mail_form'
gem 'jquery-rails', '~> 4.1', '>= 4.1.1'
gem 'dotenv-rails', groups: [:development, :test]

## bundle install

## Créer un .env

Mettre les identifiants

GMAIL_EMAIL=your_gmail_email_address
GMAIL_PASSWORD=your_gmail_password

## Mettre .env à la fin du .gitignore

# Relancer le serveur !!!!!!

## Créer à la main un model Contact

	class Contact < MailForm::Base
	  attribute :name,      :validate => true
	  attribute :email,     :validate => /\A([\w\.%\+\-]+)@([\w\-]+\.)+([\w]{2,})\z/i
	  attribute :objet			:validate => true
	  attribute :message		:validate => true
	  attribute :nickname,  :captcha  => true

	  # Declare the e-mail headers. It accepts anything the mail method
	  # in ActionMailer accepts.
	  def headers
	    {
# :subject => "Contact Form Inquiry",
# :to => "your_email@your_domain.com",
	      :from => %("#{name}" <#{email}>)
	    }
	  end
	end

## $ rails db:create si pas de base de données

## $ rails g controller contacts index create

	class ContactsController < ApplicationController
	  def index
	    @contact = Contact.new(params[:contact])
	  end

	  def create
	    @contact = Contact.new(params[:contact])
	    @contact.request = request

# avec ajax
	    respond_to do |format|
	      if @contact.deliver
	        # re-initialize Home object for cleared form
	        @contact = Contact.new
	        format.html { render 'index'}
	        format.js   { flash.now[:success] = @message = "Thank you for your message. I'll get back to you soon!" }
	      else
	        format.html { render 'index' }
	        format.js   { flash.now[:error] = @message = "Message did not send." }
	      end
	    end

# sans ajax
			if @contact.deliver
        # re-initialize Home object for cleared form
        @contact = Contact.new

        flash[:success] = "Merci pour votre message. Je reviens vers vous rapidement!"
        redirect_to root_path
      else
      	flash.now[:error] = "Votre message n'a pas pu être envoyé."
      	render :index
      end
	  end
	end


## Dans routes.rb

	Rails.application.routes.draw do
	 
	resources :contacts, only: [:index, :create]
	 
	end


## Views in contact 

### partial contact_form.html.erb

	<%= form_for @contact, url: home_index_path, remote: true do |f| %>
	  <div class="col-md-6">
	      <%= f.label :name %></br>
	      <%= f.text_field  :name, required: true, class: "contact-form-text-area" %></br>

	      <%= f.label :email %></br>
	      <%= f.text_field :email, required: true, class: "contact-form-text-area" %></br>

	    <%= f.label :message %></br>
	    <%= f.text_area :message, rows: 8, cols: 40, required: true, class: "contact-form-text-area",
	                      placeholder: "Send me a message"%></br>

	    <div class= "hidden">
	      <%= f.label :nickname %>
	      <%= f.text_field :nickname, :hint => 'Leave this field blank!' %>
	    </div>

	    <%= f.submit 'Send Message', class: 'btn btn-primary' %>
	  </div>
	<% end %>
	<div class="col-md-6" id="flash-message">
	  <%= render 'flash' %>
	</div>


### render contact_form dans contact/index.html.erb

	 <div id="contact">
	   <%= render 'contact_form' %>
	 </div>


### +/- partial flash.html.erb

	<% flash.each do |message_type, message| %>
		<div class="alert <%= bootstrap_class_for_flash(message_type) %>">
	    <%= message %>
	    	<%=button_tag class:'close', data:{dismiss:'alert'}, aria:{label:"Close"} do %>
		    <span aria-hidden="true">×</span>
		  	<% end %>
		</div>
	<% end %>

## +/- ajax: create.js.erb

	// Test for ajax success
	console.log("This is the create.js.erb file");
	// Render flash message
	$('#contact').html("<%= j render 'contact_form' %>");
	$('#flash-message').html("<%= j render 'flash' %>").delay(3000).fadeOut(4000);

## js: in application.js

	//= require jquery
	//= require jquery_ujs
	// require turbolinks
	//= require bootstrap
	//= require_tree .

## css: in contact.scss (no need if bootstrap flash)

	@import "bootstrap-sprockets";
	@import "bootstrap";

	body {
	  background-color: black;
	  color: white;
	}
	.contact-form-text-area {
	  color: black;
	}
	.hidden { display: none; }
	.alert-error {
	background-color: #f2dede;
	border-color: #eed3d7;
	color: #b94a48;
	text-align: left;
	font-size: .8em;
	}

	.alert-alert {
	background-color: #f2dede;
	border-color: #eed3d7;
	color: #b94a48;
	text-align: left;
	font-size: .8em;
	}

	.alert-success {
	background-color: #dff0d8;
	border-color: #d6e9c6;
	color: #468847;
	text-align: center;
	font-size: .8em;
	}

	.alert-notice {
	background-color: #dff0d8;
	border-color: #d6e9c6;
	color: #468847;
	text-align: left;
	font-size: .8em;
	}


## in config/environments/development.rb

	  config.action_mailer.perform_deliveries = true

		# already here
	  config.action_mailer.raise_delivery_errors= true
	  
	  config.action_mailer.delivery_method = :smtp
	  config.action_mailer.smtp_settings = {
	    address:              'smtp.gmail.com',
	    port:                 587,
	    domain:               'gmail.com',
	    user_name:            ENV["GMAIL_EMAIL"],
	    password:             ENV["GMAIL_PASSWORD"],
	    authentication:       'plain',
	    enable_starttls_auto: true  }

# Relancer le serveur !!!!!!

## Test 

https://myaccount.google.com/lesssecureapps

==> activate app less secure


## Heroku + Sendgrid

# Gmail ne marchera pas en prodution!!!!

### https://elements.heroku.com/addons/sendgrid 

$ heroku addons:create sendgrid:starter

==>  Emails per Month up to 12000 

==> crée un SENDGRID_PASSWORD et SENDGRID_USERNAME dans les config vars de heroku


### config/environments/production.rb

https://devcenter.heroku.com/articles/sendgrid#ruby

	config.action_mailer.default_url_options = { :host => 'new_app_name.herokuapp.com' }  
	config.action_mailer.delivery_method = :smtp  
	config.action_mailer.perform_deliveries = true  
	config.action_mailer.raise_delivery_errors = false   
	ActionMailer::Base.smtp_settings = {
	  :user_name => ENV['SENDGRID_USERNAME'],
	  :password => ENV['SENDGRID_PASSWORD'],
	  :domain => 'heroku.com',
	  :address => 'smtp.sendgrid.net',
	  :port => 587,
	  :authentication => :plain,
	  :enable_starttls_auto => true
	}


### $ git add . 

### $ git commit -m "App ready for first deployment"

### $ git push origin master

### $ git push heroku master

### $ heroku run bundle install

### $ heroku run rails db:migrate

### +/- $ heroku run rails db:seed



