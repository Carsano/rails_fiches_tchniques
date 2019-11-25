# Cette gem te permet de créer très facilement des models factices en mode "à la chaîne de l'usine du coin" que tu pourras appeler très facilement dans tes tests. Ainsi, ton code sera plus DRY et toute la création d'utilisateurs pour tes tests se fera dans un seul fichier.


# Installation

## Gemfile 

==> gem 'factory_bot_rails' dans envir dev et test

==> $ bundle install

## Dans spec/rails_helpers.rb

==> commenter la ligne

	config.fixture_path = "#{::Rails.root}/spec/fixtures"

==> mettre 

	config.include FactoryBot::Syntax::Methods

==> dit à rspec "hey bro, peux tu utiliser factory_bot au lieu du système de fixtures stp ?"

## Création d'un fichier factory_bot pour un Model

==> $ rails generate factory_bot:model TonModel

==> création d'un fichier spec/factories/users.rb


## Mettre dedans une sorte de seed == factory

ex: 

	FactoryBot.define do
	  factory :user do
	    first_name { "John" }
	    last_name { "Doe" }
	    username { "johndoe" }    
	  end
	end

	FactoryBot.define do
		factory :book do
		  user { FactoryBot.create(:user) }  
		end
	end


==> on pourra utiliser cette factory dans les tests 

	user = FactoryBot.create(:user)

OU MËME

	user = create(:user)

ex: test de model/before

 au lieu de 

	before(:each) do 
	  @user = User.create(first_name: "John", last_name: "Doe", username: "johndoe")
	end

 on mettra

	before(:each) do 
	  @user = FactoryBot.create(:user)
	end


## Modifications de factory

ex:

	user = FactoryBot.create(:user, first_name: "Jean")


## create/build

==> Si jamais tu ne veux pas sauvegarder ta factory en base (par exemple pour tester qu'une factory n'est pas valide), fais build et non pas create

	invalid_user = FactoryBot.build(:user, username: "aa")


==> utile dans tests du model quand l'instance test est déjà créée == utiliser build comme une sorte de update

## Création de factories à la volée avec id

		FactoryBot.define do
		  factory :article do
		    sequence(:title) { |n| "Title n°#{index}"  }
		    sequence(:content) { |n| Content n°#{index}  }
		  end
		end

## CheatSheet

https://github.com/brennovich/cheat-ruby-sheets/blob/master/factory_bot.md