	begin
	  do_something() #partie de code qui risque de produire une erreur
	rescue => e
	  puts "Oups petite erreur, mais c'est pas grave" #résultat que tu veux voir en lieu et place d'une erreur terminal
	end