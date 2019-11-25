
# Préparation du Model

## $ rails g model TonModel attribut_1:type ....

==> création du Model + migration

==> crée AUSSI le fichier de test du Model dans spec/models/ton_model_spec.rb 

## $ rails db:create (si db non créée)

## $ rails db:migrate


# Dans spec/models/ton_model_spec.rb

## A tester dans le Model

==> Réussir la création d'un model == le test va essayer de créer une instance de class

ex: 

	before(:each) do 
	  @user = User.create(first_name: "John", last_name: "Doe", username: "johndoe")
	end

==> Tester les validations des attributs == concerne les validates du Model

- Tester la validation de présence des attributs qui demandent la présence

- Tester la validation de l'unicité d'un attribut pour les attributs qui demandent l'unicité

- Etc

ex:

		context "validation" do

	    it "is valid with valid attributes" do
	      expect(@user).to be_a(User)
	      expect(@user).to be_valid
	    end

	    describe "#first_name" do
	      it "should not be valid without first_name" do
	        bad_user = User.create(last_name: "Doe")
	        expect(bad_user).not_to be_valid
	        # test très sympa qui permet de vérifier que la fameuse formule user.errors retourne bien un hash qui contient une erreur concernant le first_name. 
	        expect(bad_user.errors.include?(:first_name)).to eq(true)
	      end
	    end

	    describe "#last_name" do
	      it "should not be valid without last_name" do
	        bad_user = User.create(first_name: "John")
	        expect(bad_user).not_to be_valid
	        expect(bad_user.errors.include?(:last_name)).to eq(true)
	      end
	    end

	    describe "#username" do
	      it "should not be lower that 3 characters" do
	        invalid_user = User.create(first_name: "John", last_name: "Doe", username: "aa")
	        expect(invalid_user).not_to be_valid
	        expect(invalid_user.errors.include?(:username)).to eq(true)
	      end
	    end

	  end


==> Tester les associations == relations entre les tables si elles existent (belongs_to/has_many)

ex:

	context "associations" do

	  describe "books" do
	    it "should have_many books" do
	      book = Book.create(user: @user)
	      expect(@user.books.include?(book)).to eq(true)
	    end
	  end

	end


==> Tester les callbacks

==> Tester les méthodes d'instance

	context "public instance methods" do

	    describe "#full_name" do

	      it "should return a string" do
	        expect(@user.full_name).to be_a(String)
	      end

	      it "should return the full name" do
	        user_1 = User.create(first_name: "John", last_name: "Doe", username: "johndoe")
	        expect(user_1.full_name).to eq("John Doe")
	        user_2 = User.create(first_name: "Jean-Michel", last_name: "Durant", username: "kikou_du_75")
	        expect(user_2.full_name).to eq("Jean-Michel Durant")
	      end
	    end

	  end


==> Tester les méthodes de classe


## Modèle de test pour le Model


	require 'rails_helper'

	RSpec.describe TonModel, type: :model do

	  before(:each) do 
	  # en général, tu as envie de créer une nouvelle instance
	  end



	  context "validations" do

	    it "is valid with valid attributes" do
	      # création qui est valide
	    end

	    describe "#some_attribute" do
	      # teste cet attribut, en fonction de la validation que tu lui as donnée
	    end

	  end

	  context "associations" do

	    describe "some association" do
	      # teste cette association
	    end

	  end

	  context "callbacks" do

	    describe "some callbacks" do
	      # teste ce callback
	    end

	  end

	  context "public instance methods" do

	    describe "#some_method" do
	      # teste cette méthode
	    end

	  end

	  context "public class methods" do

	    describe "self.some_method" do
	      # teste cette méthode
	    end

	  end

	end


# Si on galère à trouver l'erreur qui ne fait pas passer tous les tests

## cf byebug.md

# DRY test

## cf factory_bot.md


