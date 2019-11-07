# clés API == Application Programming Interface

Les clés d'API sont en quelque sorte tes identifiants et mots de passe pour faire communiquer tes programmes avec un programme tiers (Twitter, Google Drive, Spotify, etc.). Le hic, c'est que si tu écris tes identifiants dans un programme que tu vas ensuite push sur GitHub (où il est public et donc lisible par tous), c'est le meilleur moyen de faire usurper ton identité sur les API.
Il existe d'ailleurs moult bots qui s'amusent à aller sur tout GitHub et récupérer les identifiants des développeurs maladroits. En les utilisant, ils peuvent alors profiter des services à ta place (surtout si ce sont des services payants) ou commettre des délits en ton nom. Sympa non ? Bienvenue sur le net.

Une interface de programmation applicative (Application Programming Interface, ou API en anglais) est un set de fonctions qui permet de communiquer avec un programme informatique. Par exemple, si l’interface web de Gmail permet d'envoyer un e-mail en cliquant sur le bouton "Envoyer", il est possible de faire la même chose en faisant un email.send via un programme Ruby connecté à l'API de Gmail (je caricature, mais c'est à peu près cela).

Les APIs sont indispensables si l'on veut faire travailler des programmes à notre place et ainsi automatiser des tâches. Par exemple, tu crées une start-up qui propose un abonnement à une box de PQ, qui envoie tous les mois une sélection des meilleurs PQ, moyennant 10 € mensuels. Tu pourrais te connecter à ton compte Stripe, puis cliquer tous les mois sur le bon bouton pour prélever de 10 € tes clients, et enfin envoyer ta super box si leur carte bleue passe. On a là une tâche sans valeur ajoutée, répétitive, que tu effectues sur un site web (autrement dit un programme informatique) : c'est typiquement le genre de chose qu'il faut automatiser par API. Laisse les bots bosser pour toi en faisant un User.all.each { |u| u.pay(10) } : ils adorent se parler entre eux.


## Gemfile

gem 'dotenv'

## $ bundle install

## Créer un fichier .env

==> y mettre les clés API

	TWITTER_API_KEY="my-twitter-api-key"
	TWITTER_API_SECRET="this_is_soooo_secret"

## Accéder aux clés depuis l'app nom_app.rp 

	require 'dotenv' # Appelle la gem Dotenv

	Dotenv.load('.env') # Ceci appelle le fichier .env (situé dans le même dossier que celui d'où tu exécute app.rb)
	# et grâce à la gem Dotenv, on importe toutes les données enregistrées dans un hash ENV

	# Il est ensuite très facile d'appeler les données du hash ENV, par exemple là je vais afficher le contenu de la clé TWITTER_API_SECRET
	puts ENV['TWITTER_API_SECRET']

#Dernière étape très importante pour rendre les clés non visibles

==> Rajouter la ligne .env dans le fichier .gitignore et le sauvegarder.


### ex: bot twitter

==> Gemfile: gem 'twitter', gem 'dotenv'

==> $ bundle install

==> dans .env

TWITTER_CONSUMER_KEY = "...."
TWITTER_CONSUMER_SECRET = "....."
TWITTER_ACCESS_TOKEN = "...."
TWITTER_ACCESS_TOKEN_SECRET = "...."


==> mettre .env dans .gitignore

==> dans le fichier de l'app


	require 'twitter'
	require 'dotenv'

	  Dotenv.load

	client = Twitter::REST::Client.new do |config|
	  config.consumer_key        = ENV["TWITTER_CONSUMER_KEY"]
	  config.consumer_secret     = ENV["TWITTER_CONSUMER_SECRET"]
	  config.access_token        = ENV["TWITTER_ACCESS_TOKEN"]
	  config.access_token_secret = ENV["TWITTER_ACCESS_TOKEN_SECRET"]
	end

	client.update('Mon premier tweet en Ruby !!!!')

