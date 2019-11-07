## Gemfile

gem 'pry' == irb dans un programme

## In the file

==> require 'pry'

==> binding.pry après ce que l'on veut tester
	

	require 'pry'

	def multiply_by_6(var) #définition d'une méthode multipliant par 6 en 2 étapes
	var = var * 2
	binding.pry # On lance PRY au milieu de la méthode
	var = var * 3
	return var
	end

	multiply_by_6(5) # j'exécute la méthode sur la valeur 5 

## Une fois le programme lancé

==> comme un irb

==> on peut appler toutes les variables et connaître leurs valeurs

==> faire des tests avec

==> modifier leurs valeurs

## pour sortir de pry

==> !!! ou exit