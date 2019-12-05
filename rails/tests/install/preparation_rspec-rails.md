
# appli rails avec postgresql

==> $ rails new -d postgresql nom_app

# Dans Gemfile == mettre gem 'rspec-rails' (sans rien derrière) dans groupe de développement et test

	group :development, :test do
	  gem 'rspec-rails'
	end

# $ bundle install

# +/- $ bundle update rspec-rails

# $ rails g rspec:install

==> install rspec-rails au sein de l'appli

- créé le fichier .rspec

- créé le dossier spec/

- créé le fichier spec/spec_helper.rb

- créé le fichier spec/rails_helper.rb


# $ rspec

==> command pour lancer les tests !!!!

# Amélioration

## Factory_bot

==> cf factory_bot.md

## Shoulda_matchers

==> cf shoulda_matchers.md
