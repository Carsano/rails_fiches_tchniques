http://intro-scrap-watir.surge.sh/

## Introduction à WATIR

Ceci est un petit tutoriel pour t'apprendre WATIR, un puissant outil qui permet d'aller récupérer des données sur des sites en simulant un utilisateur.
Pourquoi WATIR ?

Nokogiri est une gem très sympa, mais on est rapidement limité :

    Dès que l'on veut aller dans un site qui requiert de s'identifier
    Dès que l'on veut utiliser Javascript
    Enfin, si un site sait repérer facilement des requêtes HTML comme étant en provenance de robots, il aura plus de mal à repérer un robot qui navigue mine de rien sur un browser : seul le Captcha peut le détecter


## Installer Watir

### $ gem install watir ou dans Gemfile gem 'watir' + $ bundle install

### telecharger chromedriver(chrome) et/ou geckodriver(firefox)
### déziper les fichiers 
### les mettre dans /usr/bin 

	sudo mv ~/Downloads/chromedriver /usr/bin/chromedriver
	sudo mv ~/Downloads/geckodriver /usr/bin/geckodriver

### installer la gem selenium-webdriver

==> $ gem install selenium-webdriver ou dans Gemfile gem 'selenium-webdriver' + $ bundle install


## Fonctions (dans le fichier nom_app.rb)

### But du jeu == point les éléments html

### Les différents explorateurs

Chromedriver a tendance à un peu se rebeller, et balancer des erreurs à tout bout de champ, et se refermer quand le programme est fini (d'ailleurs pour solver ceci, tu peux mettre une boucle While True).

J'ai trouvé ça dans la doc qui renseigne la fonction initialize, ainsi que les paramêtres que tu peux lui passer.
https://www.rubydoc.info/gems/watir/Watir/Browser#initialize-instance_method

### la base

	require 'watir'
	# appel de la gem watir dans Gemfile

	browser = Watir::Browser.new(:firefox ou :chrome)
	# ouvre le browser firefox ou chrome

### aller sur un site

	browser.goto 'google.com'
	# dit à watir d'aller sur google.com


### Barre de recherche 

==> search_bar = browser.text_field(class: 'class_name') == pointer la barre de recherche

==> search_bar.set("ton_texte") == écrire dans la search barre 

==> ==> search_bar.send_keys(:enter) == taper la touche entrée

OU clicker sur le bouton de recherche

==> submit_button = browser.button(type:"submit") ou pointer autre chose
==> submit_button.click



### Imposer un temps de latence à watir le temps qu'une nouvelle page se charge 

==> browser.driver.manage.timeouts.implicit_wait = 3

### Pointer d'autres éléments 

==> variable_name = browser.div(class: 'class_name')
														a  (id: 'id_name')
														li (...)
														ul 
														p 
														...

### clicker

==> variable_name.click

### Récupérer plusieurs elts pointeurs identiques

==> variable_name = browser.divs(class: '...')
														lis(id: '...')
														as...

==> variable_name == array 

==> variable_name[0] == premier élément, ...

### Récupérer des données 

==> array: variable_name.each { |div| p div.h3.text } == récupère les textes de chaque p ayant comme parents div.h3

### Fermer le browser 

==> browser.close


#Exemples

## Google search "Hello wordl!" + récupération des titres des résultats de recherches


### Pour commencer, nous avons besoin de localiser le lieu où tu vas rentrer ta recherche : la fameuse barre de recherche Google. Nous allons ensuite utiliser une méthode qui permet de localiser un text_field en fonction d'un des ses attributs : search_bar = browser.text_field(attribut: 'réglage_de_lattribut'). Avec l'inspecteur d'éléments, on remarque que la barre de recherche Google a les attributs suivants :

    sa class est réglée à "gsfi"

    son id est réglée à "lst-ib"

    son name est réglé à "q"

Du coup, il suffit de renseigner l'un des paremètres suivants avec la fonction text_field.

==> search_bar = browser.text_field(class: 'gsfi')

### Niquel ! Maintenant, il va falloir penser à remplir cette case de la recherche que tu as envie de faire. Tu peux appliquer à cet élément la méthode .set. Ainsi, pour rechercher "Hello World !" tu devras rentrer la ligne suivante :

==> search_bar.set("Hello World!")

### Maintenant, on a presque fini, il te faut valider la recherche. Google a une fonction d'auto-search qui fait changer la page dès que tu fais la recherche, donc il faudra ruser. Il y a deux façons de valider la recherche : appuyer sur Entrée, ou bien cliquer sur le bouton de la loupe. Nous allons voir comment gérer les deux.

Entrée

La méthode .send_key permet de le faire. Ainsi, voici le code

==> search_bar.send_keys(:enter)

### Clic sur loupe

Il faut arriver à localiser la loupe, puis cliquer dessus. En général les boutons de submit ont comme attribut type="submit". En vérifiant avec l'inspecteur d'éléments, on remarque que c'est le cas aussi pour Google. De plus, tout comme .text_field, il existe une méthode .button qui permet de localiser les buttons.
Enfin, pour valider, il nous faudra utiliser la méthode .click

	submit_button = browser.button(type:"submit")
	submit_button.click

Je t'invite VIVEMENT à jeter un oeil sur la documentation "exemple" de Watir, ainsi que sur la documentation exhaustive de la gem.

http://watir.com/guides/index.html

https://www.rubydoc.info/gems/watir/Watir 

## Récupérer des données


### Tout d'abord, nous allons dire à notre programme d'attendre un peu avant de continuer, sinon il va faire ceci avant que la page n'ait le temps de se charger. La doc renseigne comment faire du waiting, ce qui est pile poil ce que nous cherchons.

http://watir.com/guides/waiting/

==> browser.driver.manage.timeouts.implicit_wait = 3

### Ensuite, passons à la recherche ! En checkant la doc, on remarque que l'on peut récupérer des divs et spans qui nous intéresse. C'est exactement ce qui nous intéresse. du coup testons 

==> browser.div(class:"rc")

### Comment obtenir le h3 qui nous intéresse. Essayons div.h3. Hey, ça a l'air de marcher ! Essayons d'obtenir le texte de ce h3 avec .text. Ça marche aussi, yay ! Mais malheureusement je n'ai qu'un div avec ceci, et il me faut tous les divs. Et du coup, il existe une méthode .divs qui te prend tous les divs concernés, et les met dans une array. Voici la ligne de code :

==> search_result_divs = browser.divs(class:"rc")

### Maintenant il nous reste plus qu'à passer sur les éléments de cette array et récupérer le texte :

==> search_result_divs.each { |div| p div.h3.text }

Et voilà !

Un corsaire attentif pourrait remarquer que faire directement search_result_h3s = browser.h3s(class:"r") serait plus élégant, et il a raison. J'utilise .divs pour l'exemple.

### Enfin, une fois le programme fini, tu peux fermer ton browser avec browser.close. Ton code final sera :

	require 'watir'
	browser = Watir::Browser.new()
	browser.goto 'google.com'
	search_bar = browser.text_field(class: 'gsfi')
	search_bar.set("Hello World!")

	search_bar.send_keys(:enter)

	submit_button = browser.button(type:"submit")
	submit_button.click

	browser.driver.manage.timeouts.implicit_wait = 3
	search_result_divs = browser.divs(class:"rc")
	search_result_divs.each { |div| p div.h3.text }

	p "Méfait accompli, fermeture du browser"
	browser.close


## Programme pour chercher sur google le site de thp, y accéder, se connecter et récupérer les titres de tous les cours de la formation


	require 'watir'

	browser.goto 'google.com'
	search_bar = browser.text_field(class: 'gsfi')
	search_bar.set("the hacking project")

	#méthode de la barre d'entrée
	search_bar.send_keys(:enter)

	# on dit à watir d'attendre l'affichage de la nouvelle page
	browser.driver.manage.timeouts.implicit_wait = 3

	# NOUVELLE PAGE CORRESPONDANT À LA RECHERCHE

	# pointer le lien thp
	search_thp_link = browser.a(href:"https://www.thehackingproject.org/")

	# clicker dessus
	search_thp_link.click
	
	# on attends
	browser.driver.manage.timeouts.implicit_wait = 3
	
	# liste des class login-button
	search_connexion = browser.lis(class: 'login-button')
	
	# on clique sur le btn connexion == log_in(2ème elt de l'array)
	search_connexion[1].click
	
	# on attends
	browser.driver.manage.timeouts.implicit_wait = 3
	
	# on pointe le text_field pour l'email
	email = browser.text_field(id: 'user_email')

	# on remplit
	email.set ("your_email_address")
	
	# pointe le text_field password
	password = browser.text_field(id: 'user_password')

	# on remplit le champ
	password.set ("your_password")
	
	# on pointe le bouton connexion
	btn_login = browser.button(id: 'login_session')

	#on clique
	btn_login.click
	
	#on attends
	browser.driver.manage.timeouts.implicit_wait = 3

	# on pointe les titres des cours
	courses_title = browser.divs(class: 'card-header')

	# pour chacun, on récupère le texte
	courses_title.each { |div| p div.a.text }


	# p "Méfait accompli, fermeture du browser"
	# browser.close