# Un test de vue permet juste de construire le fichier html à partir du fichier erb, puis de vérifier si tel élément existe bien en HTML. 

# Les tests de view se mettent dans le dossier spec/views/

# Préparatifs

## $ rails g rspec:view users index new

==>  créé un fichier test spec/views/users/index.html.erb

==> créé un fichier test spec/views/users/new.html.erb

# Base de fichier test

	require 'rails_helper'

	RSpec.describe "cities/index.html.erb", type: :view do

	  context 'some context' do  
	    it 'should display something' do
	      # ton test
	    end
	  end

	  context 'other context' do  
	    it 'should display something' do
	      # ton test
	    end
	  end

	end

# Example de tests

## Un test pour voir si tel mot est affiché

		context 'it says welcome' do
		  it "displays 'welcome'" do
		    # génére la page
		    render

		    # le contenu "Bievenue" doit être dans la page
		    expect(rendered).to have_content 'Bienvenue'
		  end
		end


## Un test pour vérifier que l'adresse email d'un utilisateur est bien affiché sur sa page profil

		context 'it says welcome' do
		  it "displays 'welcome'" do
		    # dit à la view que @user sera le build d'un utilisateur avec "lol@email.com" comme email
		    assign(:user, build(:user, email: "lol@email.com"))

		    # génère la vue (ceci doit être fait après avoir assigné la variable d'instance, pour qu'il puisse la trouver)
		    render
		    
		    # la vue rendered doit afficher l'email passé
		    expect(rendered).to match /lol@email.com/
		  end
		end


## Un test pour vérifier que la page index affiche bien tous les éléments

		context 'when there are cities' do  
		  it 'displays the details' do
		    # déclare la variable cities, qui est une array contenant des villes
		    assign(:cities,
		      [
		        build(:city, name: "Bordeaux"),
		        build(:city, zip_code: "34000"),
		        build(:city),
		        build(:city)
		      ]
		    )

		    render

		    # vérifie que le name d'une des villes soit affiché
		    expect(rendered).to match /Bordeaux/

		    # vérifie que le zip_code d'une ville soit affiché
		    expect(rendered).to match /34000/
		  end
		end


## Autres tests

https://relishapp.com/rspec/rspec-rails/v/3-8/docs/view-specs/view-spec