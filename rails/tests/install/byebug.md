https://github.com/deivid-rodriguez/byebug

# Installation

## $ gem install byebug

OU

## $ rails new -d postgresql ton_app met déjà la gem byebug dans le gemfile en envrinnement dev et test

OU 

## $ bundle add byebug --group "development, test"


# Usage == 2 options

## Test serveur local

==> Mettre byebug (comme binding.pry) où on veut débugger

ex: 

	def index
	  byebug
	  @articles = Article.find_recent
	end

==> $ bin/rails s

==> tester ce qu'on veut tester


## en ligne de commande

==> se déplacer dans le dossier qui contient le fichier à tester

$ byebug test_file_name.rb

==> ouvre un terminal à la même manière que pry 

==> exit pour sortir