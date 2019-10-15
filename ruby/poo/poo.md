## Création d'une class

class ClassName
end

## création d'instance de class

instance_name = ClassName.new

==> créée une instance unique avec son numéro d'identifiant!!!!

## Travailler sur une instance

### méthode d'instance

==> méthodes d'instance

class ClassName
	def method_instance
	end
end

==> création de l'instance de class

instance_name = ClassName.new

==> application de la méthode d'instance à l'instance 

instance_name.method_instance 

ex: 

	require "pry"
	class User
	  def say_hello_to_someone(first_name)
	    puts "Bonjour, #{first_name} !"
	  end
	end

	binding.pry

	jean = User.new 
	jean.say_hello_to_someone('Robert') => "Bonjour, Robert!"

### self

==> utilise l'instance elle-même!!!

	require "pry"
	class User
	  def show_itself
	    print "Voici l'instance : "
	    puts self
	  end

	end

	binding.pry

	jean = User.new 

	jean.show_itself => "Voici l'instance: id_de_l'instance_jean"


## Variables d'instance

### Définir une variable d'instance

@variable = ...

	require "pry"
	class User

	  def update_email(email_to_update)
	    @email = email_to_update
	  end

	  def read_email
	    return @email
	  end

	end

	binding.pry

	julie = User.new
	jean = User.new

	julie.read_email #cette ligne doit te retourner "nil" car par défaut, @email n'existe pas
	jean.read_email  #idem

	julie.update_email("julie@julie.com") #on change la valeur du @email de julie
	jean.update_email("jean@jean.com") #idem pour julien

	puts julie.read_email # tu devrais récupérer cette fois l'email: "julie@julie.com"
	puts jean.read_email # idem avec "jean@jean.com"

==> idée: pouvoir faire un julie.email pour récupérer l'eamil ou julie.email = "..." pour pouvoir le modifier sans passer par l'appel des méthodes d'instances!!!!

### Accès à une viariable grâce à atrr_ en haut de la class

attr_ :variable_name


==> atrr_writer == variable pas facilement lisible ==> OBLIGÉ D'APPELER LA METHODE D'INSTANCE != .variable


	require "pry"
	class User
	  attr_writer :mastercard #à mettre en en-tête de ta classe

	  def read_mastercard
	    return @mastercard
	  end
	end

	binding.pry

	julie = User.new

	julie.mastercard = "4242 4242 4242 4242" # va sauvegarder cette valeur dans la variable de l'instance julie
	julie.mastercard # retourne une erreur : on ne peut pas lire la variable facilement
	julie.read_mastercard # on a créé une méthode spécifique pour la lire. Ici ça retourne bien : => "4242 4242 4242 4242"


==> attr_reader == lire une variable d'instance simplement mais sans pouvoir la modifier facilement


	require "pry"
	class User
	  attr_reader :birthdate

	  def update_birthdate(birthdate_to_update)
	    @birthdate = birthdate_to_update
	  end
	end

	binding.pry

	julie = User.new
	julie.update_birthdate("06/01/1991") #Cette méthode permet de rattacher la date de naissance à l'instance => Date de naissance sauvegardée !

	julie.birthdate # on arrive bien à lire la variable. Ça retourne : => "06/01/1991"
	julie.birthdate = "06/01/1991" #là on a une erreur. Impossible de la modifier ainsi


==> attr_accessor == accès de le variable en ecriture et lecture!!!! == PAS BESOIN DE CRÉER UNE METHODE D'INSTANCE POUR OBTENIR LA VIABLE!!!!

	require "pry"
	class User
	  attr_accessor :email
	end

	binding.pry

	julie = User.new
	julie.email = "julie@julie.com"
	puts julie.email #=> "julie@julie.com"


## Travailler sur une class entière

### Initialize == OBLIGATOIRE!!!!!

Méthode indispensable qui permet d'exécuter du code à la création d'une instance

	require "pry"
	class User
	  attr_accessor :email

	  def initialize(email_to_save)
	    @email = email_to_save
	    puts "On envoie un email de Bienvenue !!"
	  end
	end

	binding.pry

	julie = User.new("julie@julie.com") #dès le départ, tu rattaches un email à l'instance et affiche un message
	#tu vas avoir un affichage dans le terminal : "On envoie un email de Bienvenue !!"

	julie.email #on vérifie que l’e-mail est bien enregistré. ok, ça retourne => "julie@julie.com"

	jean = User.new #tu vas avoir une erreur car tu as oublié l’e-mail => ArgumentError (wrong number of arguments (given 0, expected 1)) 


### Variable de class

Il peut être très utile de pouvoir déclarer et utiliser des variables qui concernent toute une classe. Par exemple une variable qui stocke tous tes utilisateurs créés, ou alors une variable qui les compte.
Grâce aux variables de classe tu peux faire ceci très facilement. Une variable de classe commence par @@ et est définie en en-tête de ta classe (juste après les attr_)

### Méhodes de class pour appeler les variables de class

Une méthode de classe est appelée avec NomDeLaClasse.methode (exemple: User.new). Elles sont définies de la même façon que les méthodes d'instance, sauf que tu ajoutes self. avant ton nom de méthode.

	require "pry"
	class User
	  attr_accessor :email
	  @@user_count = 0 # on initialise la variable de classe

	  def initialize(email_to_save)
	    @email = email_to_save
	    @@user_count = @@user_count + 1
	  end

	  def self.count
	    return @@user_count
	  end
	end

	binding.pry

	p User.count #=> 0
	julie = User.new("julie@julie.com")
	p User.count #=> 1
	jean = User.new("jean@jean.com")
	p User.count => 2

### Méthode privée

==> SURTOUT UTILISÉ QUAND IL S'AGIT D'UNE VÉRIFICATION, DONC TRUE OR FALSE, QUE L'ON N'A PAS BESOIN D'AFFICHER!!!!!

	require "pry"
	class User
	  def truc_public
	    # on peut faire julie.truc_public
	  end

	  private #Toutes les méthodes en dessous de cette balise seront privées. A mettre en bas de ta classe donc !

	  def truc_private
	    # impossible de faire julie.truc_private
	  end

	end

## Résumé

	class User
	  attr_accessor :attr1, :attr2
	  # chaque instance sera accompagnée de ses attributs, par ex, chaque instance de User aura un attribut nom et prénom

	  @@user_count = 0
	  # on peut initialiser une variable de class avec @@ 
	  # ici, @@user_count va compter le nombre d'instance User créée 

	  def initialize(attr1, attr2)
	  # TRES IMPORTANT == va initialier les variables de l'instance
	  # on met les attributs d'instance et les variables de class
	  # on l'utilise comme un perform == déroulé des méthodes que l'on veut appliquer
	  # pour chaque instance créée, elle sera initialisée avec tout ce qu'on met dans la method initialize

	    @attr1 = attr1
	    # initialisation d'une variable d'instance avec un @

	    @atrr2 = attr2 
	    @@user_count = @@user_count + 1
	    method_instance
			
			autre_class = AutreClass.new(User.count)
			# on peut lancer une autre création d'instance d'une autre classe qui prend en attribut User.count par ex
	  end

	  def method_instance
	  # méthode d'instance où l'on peut manipuler attr1 et/ou attr2 
	    @attr1....
	  end

	  def show_itself
	  # method appliquer à l'instance elle-même avec self 
	    ex: puts self.....
	  end

	  def self.count
	 	# method de class en self.... peut être un self.all, ...
	 	# method appliquée à la class entière
	    ex: return @@user_count
	  end

	  private
	  # pour les méthod dont on ne veut pas de sortie de résultat

	  def private_method
	    return "##ENCRYPTED##"
	  end

	end

	user = User.new(attr1, attr2)

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

##### la Class2 peut avoir ses propres attributs

	class Class1 < Class2
		attr_accessor :attr_Class1

		def initialize(attributs, de, la, Class2, attr_Class1)
			@attr_Class1 = attr_Class1
			super(attributs, de, la, Class2)
		end

		(...) #reste du code de la classe

	end




