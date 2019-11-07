## Création d'une class

class ClassName
end

## création d'instance de class

# name = ClassName.new

==> créée une instance unique avec son numéro d'identifiant!!!!

## Travailler sur une instance

### méthode d'instance

==> méthodes d'instance

	class ClassName
		def method_instance
		end
	end

==> création de l'instance de class

name = ClassName.new

==> application de la méthode d'instance à l'instance 

# name.method_instance 

ex: 

	class User
	# nom de la class

	  def say_hello_to_someone(first_name)
	  # méthode d'instance pour chaque instance de class créée
	    puts "Bonjour, #{first_name} !"
	  end
	end

	jean = User.new
	# on créé l'objet == instance de class, unique  
	jean.say_hello_to_someone('Robert') => "Bonjour, Robert!"
	# on applique à l'objet jean, la méthode d'instance say_hello_to_someone(first_name)


### self

==> utilise l'instance elle-même!!!

	class User

	  def show_itself
	    print "Voici l'instance : "
	    puts self
	    # self == l'instance créée
	  end

	end

	jean = User.new 
	jean.show_itself => "Voici l'instance: id_de_l'instance_jean"


## Variables d'instance

### Définir une variable d'instance

variable d'instance == les attributs avec lesquels l'instance est créée

@variable = ...

permet d'utiliser cette variable sans devoir l'appeler () dans les méthodes d'instance

permet d'accéder à cette variable en faisant un instance_créée.variable_d'instance

	class User

	  def update_email(email_to_update)
	  # on récupère l'email à updater lors de la création de l'instance User
	    @email = email_to_update
	    # on fixe email_to_update en variable d'instance @email
	  end

	  def read_email
	    return @email
	    # permet d'injecter @email directement dans la méthode read_email au lieu de read_email(email_to_update)
	  end

	end

	julie = User.new
	julie.read_email #cette ligne doit te retourner "nil" car par défaut, @email n'existe pas
	julie.update_email("julie@julie.com") #on change la valeur du @email de julie

	puts julie.read_email # tu devrais récupérer cette fois l'email: "julie@julie.com"


#Grâce à @email, on peut faire maintenant un julie.email pour récupérer l'eamil ou julie.email = "..." pour pouvoir le modifier sans passer par l'appel des méthodes d'instances!!!!


### Accès à une variable grâce à atrr_ en haut de la class

# evite de faire une méthode de récupération des attributs de l'instance (ex: def update_email(email_to_update) avant)
# == les variables auxquelles on veut pouvoir accéder!!!!!!
# ex: ClassName.variable_name =

attr_ :variable_name

3 types d'attr_

==> atrr_writer == variable pas facilement lisible ==> OBLIGÉ D'APPELER LA METHODE D'INSTANCE != .variable


	class User
	  attr_writer :mastercard #à mettre en en-tête de ta classe
	  # on annonce que l'instance user va avoir comme atrribut mastercard, qui ne va être que écrivable

	  def read_mastercard
	    return @mastercard
	    # on fixe l'attribut en @variable d'instance
	  end
	end

	julie = User.new

	julie.mastercard = "4242 4242 4242 4242" # va sauvegarder cette valeur dans la variable de l'instance julie
	julie.read_mastercard # on a créé une méthode spécifique pour la lire. Ici ça retourne bien : => "4242 4242 4242 4242"

	Par contre
	julie.mastercard # retourne une erreur : on ne peut pas lire la variable facilement


==> attr_reader == lire une variable d'instance simplement mais sans pouvoir la modifier facilement


	class User
	  attr_reader :birthdate
	  # on annonce que l'instance user va avoir comme atrribut mastercard, qui ne va être que lisible

	  def update_birthdate(birthdate_to_update)
	  # si elle n'est que lisible, il va falloir rentrer l'attribut via cette méthode d'instance

	    @birthdate = birthdate_to_update
	    # on fixe l'attribut en @variable
	  end
	end

	julie = User.new
	julie.update_birthdate("06/01/1991") #Cette méthode permet de rattacher la date de naissance à l'instance => Date de naissance sauvegardée !
	julie.birthdate # on arrive bien à lire la variable. Ça retourne : => "06/01/1991"

	Par contre
	julie.birthdate = "06/01/1991" #là on a une erreur. Impossible de la modifier ainsi


==> attr_accessor == accès de le variable en ecriture et lecture!!!! == PAS BESOIN DE CRÉER UNE METHODE D'INSTANCE POUR OBTENIR LA VARIABLE!!!!


	class User
	  attr_accessor :email
	  # attribut lisible et écrivable associé à l'instance user
	end

	julie = User.new
	julie.email = "julie@julie.com"
	puts julie.email #=> "julie@julie.com"


## Travailler sur une class entière

### Initialize == OBLIGATOIRE!!!!!

Méthode indispensable qui permet d'exécuter du code à la création d'une instance == initialise la l'instance avec ses variables

	class User
	  attr_accessor :email
	  # l'instance user créée aura comme attribut un email

	  def initialize(email_to_save)
	  # cette attribut sera à mettre lors de la création de l'instance == User.new(nom_email_to_save)

	    @email = email_to_save
	    # on fixe l'attribut en @variable pour pouvoir l'injecter dans les méthodes d'instance
	    puts "On envoie un email de Bienvenue !!"
	  end
	end

	julie = User.new("julie@julie.com") 
	#dès le départ, tu rattaches un email à l'instance et affiche un message
	==> dans le terminal : "On envoie un email de Bienvenue !!"

	julie.email
	#on vérifie que l’e-mail est bien enregistré. ok, ça retourne => "julie@julie.com" (partie reader de l'attr)

	julie.email = "autre_email"
	# changement d'email (partie writer de l'attr)
	
	Par contre
	jean = User.new #tu vas avoir une erreur car tu as oublié l’e-mail => ArgumentError (wrong number of arguments (given 0, expected 1)) 


### Variable de class

Il peut être très utile de pouvoir déclarer et utiliser des variables qui concernent toute une classe. Par exemple une variable qui stocke tous tes utilisateurs créés, ou alors une variable qui les compte.
Grâce aux variables de classe tu peux faire ceci très facilement. Une variable de classe commence par @@ et est définie en en-tête de ta classe (juste après les attr_)

### Méhodes de class pour appeler les variables de class

Une méthode de classe est appelée avec NomDeLaClasse.methode (exemple: User.new, User.all, ...). Elles sont définies de la même façon que les méthodes d'instance, sauf que tu ajoutes self. avant ton nom de méthode.

	class User
	  attr_accessor :email
	  # l'instance de class aura un attribut email en écriture et lecture

	  @@user_count = 0 
	  # on initialise la variable de classe == @@

	  def initialize(email_to_save)
	    @email = email_to_save
	    # on fixe l'attr en @variable d'instance
	    @@user_count = @@user_count + 1
	    # la @@variable de class va compter +1 à chaque création d'instance de class
	  end

	  def self.count
		# méthode de class (self)
	    return @@user_count
	    # avec sa variable de class
	  end
	end

	p User.count #=> 0
	julie = User.new("julie@julie.com")
	p User.count #=> 1
	jean = User.new("jean@jean.com")
	p User.count => 2

### Méthode privée

==> SURTOUT UTILISÉ QUAND IL S'AGIT D'UNE VÉRIFICATION, DONC TRUE OR FALSE, QUE L'ON N'A PAS BESOIN D'AFFICHER!!!!!

	class User
	  def method_public
	   # on peut faire julie.method_public
	  end

	  private #Toutes les méthodes en dessous de cette balise seront privées. A mettre en bas de ta classe donc !

	  def method_private
	   # impossible de faire julie.method_private
	  end

	end


## Résumé

	class User
	  attr_accessor :attr1, :attr2
	  # chaque instance sera accompagnée de ses attributs, par ex, chaque instance de User aura un attribut nom et un attribut prénom

	  @@user_count = 0
	  # on peut initialiser une variable de class avec @@ 
	  # ici, @@user_count va compter le nombre d'instance User créée 

	  def initialize(attr1, attr2)
	  # TRES IMPORTANT == va initialiser l'instance avec ses attributs (nom, prenom)
	  # pour chaque instance créée, elle sera initialisée avec tout ce qu'on met dans la method initialize
	  # sorte de kit de départ de l'instance
	  # on y met les attributs d'instance et les variables de class

	    @attr1 = attr1
	    # initialisation d'une variable d'instance avec un @ qui correspond à l'attr1 (nom)

	    @atrr2 = attr2 
	    # initialisation d'une variable d'instance avec un @ qui correspond à l'attr2 (prénom)

	    @@user_count = @@user_count + 1
	    # initialisation de la @@variable de class
			
			method_instance
			show_itself
			# on peut l'utiliser comme un perform == déroulé des méthodes que l'on veut appliquer

			autre_class = AutreClass.new(User.count)
			# on peut lancer une autre création d'instance d'une autre classe qui prend en attribut User.count par ex
			autre_class = AutreClass.new
			# on peut lancer une autre création d'instance d'une autre classe qui ne prend pas d'attribut

	  end

	  def method_instance
	  # méthode d'instance où l'on peut manipuler attr1 et/ou attr2 
	    @attr1....
	  end

	  def show_itself
	  # method appliquer à l'instance elle-même avec self 
	    ex: puts self.attr1
	  end

	  def self.count (ou self.all, ...)
	 	# method de class en self....
	 	# method appliquée à la class entière
	    ex: return @@user_count
	  end

	  private
	  # pour les méthod dont on ne veut pas de sortie de résultat

	  def private_method
	    ...
	  end

	end

	user = User.new(attr1, attr2)
	# le initialize prend deux attributs

	# Une fois l'instance créé, on peut la manipuler comme on veut
	user.attr1 == sort attr1
	user.attr2 == "attr3" ==> on peut changer la valeur de l'attribut
	user.attr2 ==      attr2 
	user.method_instance == on peut lui appliquer une method 

	User.count == on applique la method_class à la class User
	User.all ....


## Dans le cas de plusieurs class imbriquées

### On peut réinjecter une instance de class dans les attributs d'une autre

	julie = User.new("julie@email.com", 35) #je crée un premier User
	jean = User.new("jean@maimail.com", 22) #puis un second User

	my_event = Event.new("2019-01-13 09:00", 10, "standup quotidien", [julie, jean]) #et je les insère tous les deux dans un nouvel Event
	# ce qui implique que la class Event prend en attribut une date, une durée, un titre, des participants

	class Event
		attr_accessor :date, :duration, :title, :attendees
		# si attendees peut contenir plusieurs user, attendees va être initialisé en array

		def initialize(the_date, duration, the_title, [user1, user2])
			@date = the_date
			# initialisation de la @variable d'instance @date
			@duration = duration
			@title = the_title
			@attendees = [user1, user2]
		end
	end

### HÉRITAGE

#### class Class1 < Class2 == HÉRITAGE: la Class1 va hérité de toute la Class1 == variables, methode,....

	class Class1 < Class2
	end

#### Class1 SANS def initialize
	
	class Class1 < Class2
		def method
			# on peut injecter les variables de Class2
		end
	end

	class2 = Class2.new(attr_Class1) 

	class1 = Class1.new(attr_Class1)

	on peut appliquer la method à class1 != class2

	class1.method != class2.method

	# N'a pas grand intérêt

#### Class1 AVEC def initialize

##### on doit réinitialiser les variables de la Class2 dans le initialize de la Class1 

==> #super == permet d'appeler les variables de la Class2 en une ligne

	class Class1 < Class2
		def initialize(attributs, de, la, Class2)
			super(attributs, de, la, Class2)
		end

		(...) #reste du code de la classe

	end

##### la Class1 peut avoir ses propres attributs

	class Class1 < Class2
		attr_accessor :attr_Class1

		def initialize(attributs, de, la, Class2, attr_Class1_ou_pas)
			@attr_Class1 = attr_Class1
			super(attributs, de, la, Class2)
		end

		(...) #reste du code de la classe

	end




