# *********** OMNIAUTH **************


https://www.grafikart.fr/tutoriels/devise-omniauth-859


Permet de se connecter avec compte facebook, google, twitter...

==> on utilisera leur nom et mail du réseau social

S'intègre très bien à devise

Step pour facebook

## Gemfile

gem 'omniauth'
gem 'omnniauth-facebook'

## $ bundle install

## initializers devise 

décommenter config.omniauth

## https://github.com/omniauth/omniauth/wiki/List-of-Strategies 

choisir sa stratégie == réseau social

## https://github.com/mkdynamic/omniauth-facebook

## chercher les providers fb (section Usage)

==> fb key 

==> fb secret

## les mettres dans initializers

		config.omniauth :facebook, 'APP_ID', 'APP_SECRET', scope: 'user, public_repo'

en troisième paramètre, on peut ajouter d'autres choses:

(section configuration)

=> infos supplémentaires profil utilisateur, etc


## Demander ses clés API à FB !!!!!!!

## Model User 

==> rajouter les providers à Devise 

	devise :omniauthable, omniauth.providers: [:facebook]
	  #si plusieurs providers, on les rajoute dans l'array avec facebook


## Creétion de nouvelles routes

$ rails routes

## Créer un nouveau controller dans un dossier users

controllers/users/omniauth_callbacks_controller.rb


class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

	def facebook
	end 

end

## Dire à Devise d'utiliser ce controller 

==> dans routes.rb

devise_for :users, controllers: {
	omniauth_callbacks: 'users/omniauth_callbacks'}

## retour au controller créé


	class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

		def facebook
			request.env['omniauth.auth']
		end 

	end


## Rajouter le bouton connexion fb dans formulaire de connexion devise

<%= link_to 'Se connecter via Facebook', user_facebook_omniauth_authorize_path %>

## Retour au controller

	class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

		def facebook
			@user = User.from_facebook(request.env['omniauth.auth'])

			if @user.persisted?
				sign_in_and_redirect @user, event: authentication
			else
				session['devise.facebook'] = request.env['omniauth.auth']
				# permet de récup les données pour préremplir les champs
				redirect_to new_user_registration_url
			end
		end 

	end

## Créer la méthode from_facebook dans model User

	def self.from_facebook(auth)
		# dire de récuprer l'utilisateur fb avec l'id récup par api fb

	end

==> pb: RIEN dan la db !!!!!

==> couper le serveur

==> rollback migration de devise

		## Omniauthable
		t.string :facebook_id
		t.string :google_id

==> rdm

## Retour au model et def facebook

	def self.from_facebook(auth)
		# dire de récuprer l'utilisateur fb avec l'id récup par api fb
		where(facebook_id: auth.uid).first_or_create do |user|
			user.email = auth.info.email
			user.username = auth.info.name
			user.password = Devise.friendly_token[0, 20]
			# aléatoire
			user.skip_confirmation!
		end

	end




# PointBudget omniauth

## Gemfile

	gem 'omniauth'
	gem 'omniauth-facebook'

Public/initializers/devise.rb 

	# config.omniauth :facebook, ENV['FACEBOOK_KEY'], ENV['FACEBOOK_SECRET']


## Model

		devise :omniauthable, omniauth_providers: [:facebook]

		  For omniauth faceboook
		  # def self.new_with_session(params, session)
		  #   super.tap do |user|
		  #     if data = session["devise.facebook_data"] && session["devise.facebook_data"]["extra"]["raw_info"]
		  #       user.email = data["email"] if user.email.blank?
		  #     end
		  #   end
		  # end

		  For omniauth faceboook
		  # def self.from_omniauth(auth)
		  #   where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
		  #     user.email = auth.info.email
		  #     user.password = Devise.friendly_token[0,20]
		  #   end
		  # end


## routes.rb

==> virer omniauth

	devise_for :users, controllers: { registrations: :registrations, ,
                                    omniauth_callbacks: "users/omniauth_callbacks" }


## create controllers/users/omniauth_callbacks_controllers.rb

	class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
	  def facebook
	    @user = User.from_omniauth(request.env["omniauth.auth"])

	    if @user.persisted?
	      sign_in_and_redirect @user, :event => :authentication
	      set_flash_message(:notice, :success, :kind => "Facebook") if is_navigational_format?
	    else
	      session["devise.facebook_data"] = request.env["omniauth.auth"]
	      redirect_to new_user_registration_url
	    end
	  end

	  def failure
	    redirect_to root_path
	  end
	end


## migration add omniauth to users 

	class AddOmniauthToUsers < ActiveRecord::Migration[5.2]
	  def change
	    add_column :users, :provider, :string
	    add_column :users, :uid, :string
	  end
	end

## dans devise/shared/_links


	<%- if devise_mapping.omniauthable? %>
	  <%- resource_class.omniauth_providers.each do |provider| %>
	    <%= link_to omniauth_authorize_path(resource_name, provider), id:"spinner" do %>
	      <i class=" fa-2x fa-facebook-f fa-lg fab white-text pt-2 pr-2"> </i>Connexion <%=OmniAuth::Utils.camelize(provider)%>
	    <%end%>
	  <% end %>
	<% end %>


