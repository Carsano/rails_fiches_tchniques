
## gem install rspec OU gem 'rspec' dans Gemfile

## $ bundle install

## $ rspec --init

==> création d'un dossier spec

==> création d'un fichier spec_helper.rb

==> création d'un .rspec

## créer dans /lib le fichier ruby

==> nom_fichier.rb

## créer dans /spec le fichier spec

==> nom_fichier_spec.rb


	require_relative '../lib/nom_fichier'

	describe "nom_methode function" do
	  it "ce_qu_elle_fait" do
	    expect(nom_method).to eq(ce_qu_elle_sort)
	  end
	end


## Pour tester

==> rpsec == teste l'ensemble des spec

==> rspec nom_fichier_spec.rb == teste un fichier
