
## Boucle For ... in (1..n)

La boucle for .. in ... , elle, permet également de répéter plusieurs fois des instructions comprises entre for et end mais en disposant d'une variable qui joue le rôle de "compteur". 
Ce compteur est incrémenté à chaque tour de boucle. Voici un petit exemple avec une variable qu'on appelle count.

	for count in (1..5)
		puts count
	end


## Boucle n.times

La boucle n.times va permettre d’exécuter n fois l'ensemble des instructions contenues dans la boucle, soit entre le n.times et le end.

	7.times do
	  puts "hello world!"
	end


## Boucle each (boucle de référence pour les array)

	array_name.each do |elt_array|
	end

## Boucles while

S'arrête en fonction d'une condition (un booléen qui doit passer de true à false).

	while condition
	end


#Utilise les boucles while à bon escient. Il vaut mieux toujours privilégier les boucles for / each / times qui sont plus simples et ne peuvent pas conduire à une boucle infinie.

	==> Assure-toi que la condition d'arrêt de ta boucle while va passer en false à un moment donné.

	==> En cas de doute, mets des puts en début et fin de boucle pour afficher les variables de la condition d'arrêt : ça te permettra de comprendre comme celles-ci évoluent et si un false peut arriver.
