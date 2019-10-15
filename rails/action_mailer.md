

# ********* ACTION MAILER ********


Rails dispose d'un outil de gestion des envois d'e-mail plutôt bien conçu : Action Mailer. Grâce à lui, tu vas pouvoir automatiser l'envoi de certains e-mails selon les critères que tu définiras (actions de tes utilisateurs, événements ou alertes données, etc..).

# Résumer commandes

## $ rails g mailer UserMailer
==> crée un ficher user_mailer.rb dans app/mailers == definition du mail à envoyer

==> crée un dossier user_mailer dans app/views == visuel dans mail en html et text

## Définir à quel moment notre app Rails doit effectuer l'envoi
==> Model, Controller == méthode + callback de ces méthodes

## Config ActionMailer

==> en dev:

- installation de la gem 'letter_opener' dans Gemfile
- $ bundle install
- Colle les lignes config.action_mailer.delivery_method = :letter_opener et config.action_mailer.perform_deliveries = true dans config/environments/development.rb

# Devise: ajouter config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

- dans le terminal, faire une création d'instance
- Tu devrais voir un visuel de l’e-mail que tu as rédigé en HTML s'afficher dans ton navigateur !

==> en prod:

- choisir un service d'envoi (ex: Sengrid)
- sauvegarder les clés API
- paramétrer le SMTP avec Sengrid
- $ heroku config:set SENDGRID_PWD='clé_api'
- test avec yopmail.com



L'Action Mailer est organisé en plusieurs éléments au sein d'une app Rails :

    => Des mailers, qui sont ni plus ni moins que des sortes de controllers appliqués aux e-mails. Tout comme les controllers "normaux", ils contiennent des méthodes qui vont faire des appels à la BDD (via les models) et ensuite envoyer des e-mails (au lieu d'envoyer des pages web à des navigateurs).
    => Des views, qui sont des sortes de templates des e-mails à envoyer. Tout comme les views de ton site, elles sont personnalisées grâce à du code Ruby inclus dedans (pour rajouter un nom, un e-mail, le contenu d'un objet récupéré en BDD, etc.). Il existe deux types de views : les .text.erb et les .html.erb car on peut envoyer des e-mails au format HTML comme au format text.

Au final, il faut considérer qu'Action Mailer a un fonctionnement très proche du MVC classique de Rails sauf qu'au lieu d'afficher des pages HTML sur un navigateur, il envoie des fichiers HTML ou text par e-mail.

# 1) Mise en place de Action Mailer

## $ rails g mailer UserMailer

### crée un ficher user_mailer.rb dans app/mailers

Tu vas y mettre la definition du mail à envoyer

Ex avec un email de bienvenue lors de l'inscription sur l'appli 

	class UserMailer < ApplicationMailer
	  default from: 'no-reply@monsite.fr'
	 
	  def welcome_email(user)

	    #on récupère l'instance user pour ensuite pouvoir la passer à la view en @user
	    @user = user 

	    #on définit une variable @url qu'on utilisera dans la view d’e-mail
	    @url  = 'http://monsite.fr/login' 

	    # c'est cet appel à mail() qui permet d'envoyer l’e-mail en définissant destinataire et sujet.
	    mail(to: @user.email, subject: 'Bienvenue chez nous !') 

	  end
	end

### crée un dossier user_mailer dans app/views

Création d'un template d'email dans app/views/user_mailer/

#### IMPORTANT: le nom du template est extrêmement important. Il doit être identique à celui de la méthode welcome_email et placé dans le dossier views/user_mailer/ qui contient tous les templates e-mails relatifs au mailer UserMailer

-template de type html : welcome_email.html.erb

	<!DOCTYPE html>
	<html>
	  <head>
	    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type' />
	  </head>
	  <body>
	    <h1>Salut <%= @user.name %> et bienvenue chez nous !</h1>
	    <p>
	      Tu t'es inscrit sur monsite.fr en utilisant l'e-mail suivant : <%= @user.email %>.
	    </p>
	    <p>
	      Pour accéder à ton espace client, connecte-toi via : <%= @url %>.
	    </p>
	    <p> À très vite sur monsite.fr !
	  </body>
	</html>

-template de type text: welcome_email.txt.erb

	Salut <%= @user.name %> et bienvenue chez nous !
	==========================================================
	 
	Tu t'es inscrit sur monsite.fr en utilisant l'e-mail suivant : <%= @user.email %>.
	 
	Pour accéder à ton espace client, connecte-toi via : <%= @url %>.
	 
	À très vite sur monsite.fr !


## Définir à quel moment notre app Rails doit effectuer l'envoi

### Généralement, c'est au modèle de le faire: définir une méthode appelant la méthode définit dans UserMailer à appliquer après création de l'instance du model == callback after_create

Si tu veux envoyer un email à la création d'un utilisateur, c'est un callback after_create dans le model User

Si tu veux envoyer un email quand un utilisateur vient de prendre un RDV sur Doctolib, c'est un callback after_create à la création d'un Appointment

	class User < ApplicationRecord
	  after_create :welcome_send

	  def welcome_send
	    UserMailer.welcome_email(self).deliver_now
	  end

	end

### Des fois, dans le controller

ex: Un email pour réinitialiser le mot de passe peut se mettre dans le controller

### autres cas: création de Service
ex: Si tu veux envoyer une newsletter hebdomadaire, c'est un Service qui tourne de manière hebdomadaire
## Définir à quel moment notre app Rails doit effectuer l'envoi

# 2) Résumé

    - Un utilisateur est créé en BDD par le model
    - Grâce au callback after_create, on exécute la méthode welcome_send sur l'instance qui vient d'être sauvée en BDD
    - welcome_send dit, en résumé, "exécute NOW la méthode welcome_email située dans le mailer UserMailer"
    - welcome_email va appeler 2 templates en leur mettant à disposition une instance @user qui est l'utilisateur créé et une variable @url qui est juste un string. Cette méthode enverra ensuite les 2 templates à - @user.email avec comme sujet "Bienvenue chez nous".
    - Les 2 templates (un HTML et un text) sont personnalisés avec les entrées en Ruby (@user.name, @user.email et @url) avant d'être balancés par e-mail


# 3) Configuration de Action Mailer

On veut pouvoir envoyer des vrais e-mails. C'est tout.

Pour le faire, tu as le choix entre plein de services différents : Mandrill by MailChimp, Postmark, Amazon SES, etc. Nous, on a une préférence pour MailJet à THP (ils sont efficaces, pas chers et français).
Toutefois, pour des raisons de fiabilité d’envoi depuis des adresses gratuites ou fake, je vais te montrer comment passer par le leader du secteur : SendGrid.

## Config en development

### installation de la gem 'letter_opener' dans Gemfile

### $ bundle install

### dans config/environments/development.rb (fichier contenant les paramètres de ton environnement de développement)

Colle les lignes config.action_mailer.delivery_method = :letter_opener et config.action_mailer.perform_deliveries = true

### dans le terminal, faire une création d'instance

### Tu devrais voir un visuel de l’e-mail que tu as rédigé en HTML s'afficher dans ton navigateur !


## Config en production

Tu veux pouvoir tester l'affichage de l’e-mail mais éviter de spammer les utilisateurs avec tes tests.
Pour le faire, tu as le choix entre plein de services différents : Mandrill by MailChimp, Postmark, Amazon SES, etc. Nous, on a une préférence pour MailJet à THP (ils sont efficaces, pas chers et français).
Toutefois, pour des raisons de fiabilité d’envoi depuis des adresses gratuites ou fake, je vais te montrer comment passer par le leader du secteur : SendGrid.

### choisir un service d'envoi (ex: Sengrid)

	- créer un compte sur https://signup.sendgrid.com/ : indique un site web bidon, précise que tu es développeur et dis que tu vas utiliser leur SMTP/API.
	- https://app.sendgrid.com/guide/integrate/langs/smtp pour créer une clef API.

### sauvegarder les clés API

	  - Crée un fichier .env à la racine de ton application.
	  - Ouvre-le et écris dedans les informations suivantes : SENDGRID_LOGIN='apikey' et SENDGRID_PWD='ta_clef_API' en remplaçant bien sûr ta_clef_API par la clef que tu viens de générer.
	  - Rajoute gem 'dotenv-rails' à ton Gemfile et fait le $ bundle install
	  - Et l'étape cruciale qu'on oublie trop souvent : ouvre le fichier .gitignore à la racine de ton app Rails et écris .env dedans.

### Dans config/environnement/production.rb

Mettre: config.action_controller.default_url_options = {:host => "app_name"}
### paramétrer le SMTP avec Sengrid

Dans /config/envirronement.rb, rajouter

	ActionMailer::Base.smtp_settings = {
	  :user_name => ENV['SENDGRID_LOGIN'],
	  :password => ENV['SENDGRID_PWD'],
	  :domain => 'monsite.fr',
	  :address => 'smtp.sendgrid.net',
	  :port => 587,
	  :authentication => :plain,
	  :enable_starttls_auto => true
	}

## Passer les clefs API à Heroku

### $ heroku config:set SENDGRID_PWD='clé_api'

## Test d'envoi

### dans un premier temps, tu peux faire plus simple en testant une fois l'envoi directement depuis l'environnement de développement (ton ordi).

    - Enlever la ligne config.action_mailer.delivery_method = :letter_opener du fichier config/environments/development.rb ;
    - Va dans ta console Rails ;
    - Créé un utilisateur avec une adresse en @yopmail.com ;
    - Va vérifier que l’e-mail est bien arrivé sur http://www.yopmail.com/.

ATTENTION: 

	Ces services d'envois en masse ont été conçus pour envoyer des e-mails depuis des domaines propriétaires. Si tu ne possèdes pas un nom de domaine (genre "thehackingprojet.org"), tu vas devoir utiliser un destinataire soit fake ("no-reply@monsite.fr") soit gratuit (@yahoo ou @gmail). Dans les deux cas, tes e-mails vont être vite considérés comme du spam et tout simplement rejetés par la majorité des serveurs e-mails…

	Seule solution pour tester ton code : viser des adresses du genre @yopmail.com qui sont habituées à servir de poubelle et du coup, elles acceptent tout !

### en production, envoi de mails non considérés comme des spams

- Comme je te l'ai expliqué, la solution propre, c'est d'acheter un nom de domaine et de le paramétrer dans SendGrid…

- Une autre solution, qui n'est pas applicable pour une "vraie" société, est de ne pas passer par SendGrid mais directement par la configuration SMTP de ton adresse mail perso. Par exemple, pour envoyer des e-mails via ton adresse Gmail, il te faut remplacer la configuration SMTP de Sendgrid par les lignes suivantes dans /config/environment.rb

		ActionMailer::Base.smtp_settings = {
			:user_name => ENV['GMAIL_LOGIN'],
			:password => ENV['GMAIL_PWD'],
			:domain => 'gmail.com', 
			#you can also use google.com
			:address => 'smtp.gmail.com',
			:port => 587,
			:authentication => :plain,
			:enable_starttls_auto => true  
		}

ATTENTION: Évidemment, il faut que tu rajoutes dans ton fichier .env ton login Gmail et ton mot de passe sous la forme ENV['GMAIL_LOGIN'] = 'jose@gmail.com' et ENV['GMAIL_PWD'] = 'p1rouette_KKouette'.



 =============================================================================
 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 =============================================================================


# EXEMPLE: ENVOI MAIL DE CONFIRMATION DE RECEPTION À UNE PERSONNE, APRÈS LE REMPLISSAGE D'UN FORMULAIRE DE CONTACT

## $ rails new -d postgresql les_voix_se_levent

## Gemfile: 'pry', 'letter_opener', 'table_print', 'faker'

## $ bundle install

## $ rails g model User first_name:string last_name:string email:string title:string content:string

==> crétion d'un model User

	class User < ApplicationRecord
		validates :first_name, presence: true
		validates :last_name, presence: true
		validates :email, presence: true, { with: /\A[^@\s]+@([^@\s]+\.)+[^@\s]+\z/, message: "email adress please" } 
		validates :title, presence: true
		validates :description, presence: true
	end

==> création d'une migration create_users.rb dans /db/migrate

==> $ rails db:migrate

## $ rails g controller users index new create 

==> création d'un controller users avec les différentes méthodes dans /app/controllers

	class EventsController < ApplicationController

		def index
		end

	  def new
	  end

	  def create

	    puts "#"*60
	    puts params
	    puts "#"*60
	    
	    @user = User.new(first_name:params[:first_name], last_name:params[:last_name], title:params[:title], description:params[:description])

	    if @user.save 

	    flash[:success] = "Votre demande de contact a bien été prise en charge."
	    redirect_to users_path

	    else

	    render 'new'

	    end
	  end
	  
	end

==> création des views correspondantes aux méthodes dans /app/views/users

==> création des routes dans /config: penser à les mettre en resources :users

==> crétion des fichiers .scss et .coffee correspondantes

## $ rails g mailer UserMailer

==> dans app/mailer/user_mailer.rb

	class UserMailer < ApplicationMailer
	  default from: 'no-reply@les_voix_se_levent.fr'
		def contact_email(user)
	    @user = user 
	    # @url  = 'http://www.lesvoixselevent.fr/ 
	    mail(to: @user.email, subject: 'Réception de votre email')
	  end
	end

==> dans app/views/user_mailer

-template user_mailer.html.erb 

	<!DOCTYPE html>
	<html>
	  <head>
	    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type' />
	  </head>
	  <body>
	    <h1>Bonjour <%= @user.last_name %> <%= user.last_name %></h1>
	    <p>
	      Vous venez de remplir le formulaire de contact concernant l'exposition photosonore "Les Voix se Lèvent", en utilisant l'e-mail suivant: <%= @user.email %>.
	    </p>
	    <p>
	    	Nous accusons réception de votre demande de contact et nous vous répondrons dans les plus brefs délais.
	    </p>
	    <p>
	    	A très bientôt.
	    </p>
	  </body>
	</html>

-template user_mailer.text.erb 

	Bonjour <%= @user.last_name %> <%= user.last_name %>
		==========================================================
		 
		Vous venez de remplir le formulaire de contact concernant l'exposition photosonore "Les Voix se Lèvent", en utilisant l'e-mail suivant: <%= @user.email %>.
		 
		Nous accusons réception de votre demande de contact et nous vous répondrons dans les plus brefs délais.

		A très bientôt.

## Dans le model User, mettre le callback after_create

	class User < ApplicationRecord

		after_create :contact_email

		def contact_email
	    UserMailer.contact_email(self).deliver_now
	  end

		validates :first_name, presence: true
		validates :last_name, presence: true
		validates :email, presence: true, { with: /\A[^@\s]+@([^@\s]+\.)+[^@\s]+\z/, message: "email adress please" } 
		validates :title, presence: true
		validates :description, presence: true

	end

## Config Action Mailer

### Test en development

==> dans config/environments/development.rb: config.action_mailer.delivery_method = :letter_opener et config.action_mailer.perform_deliveries = true

==> dans le terminal, faire une création d'instance genre User.create(............)

==> Tu devrais voir un visuel de l’e-mail que tu as rédigé en HTML s'afficher dans ton navigateur !

### Production

#### test avec sendgrid

==> mettre clés API dans .env

==> dans /config/envirronement.rb

	ActionMailer::Base.smtp_settings = {
	  :user_name => ENV['SENDGRID_LOGIN'],
	  :password => ENV['SENDGRID_PWD'],
	  :domain => 'monsite.fr',
	  :address => 'smtp.sendgrid.net',
	  :port => 587,
	  :authentication => :plain,
	  :enable_starttls_auto => true
	}

==> Enlever la ligne config.action_mailer.delivery_method = :letter_opener du fichier config/environments/development.rb 

==> test en console avec une adresse en @yopmail.com

==> $ heroku config:set PROTONMAIL_PWD='clé_api'

==> Sengrid + yopmail.com

#### test avec boîte mail perso

==> chercher clé API boîte mail == nom d'utilisateur/mdp

==> mettre ces clés dans .env

PROTONMAIL_LOGIN='nom_utilisateur'
PROTONMAIL_PWD='mot_de_passe'

==> Dans /config/envirronement.rb

	ActionMailer::Base.smtp_settings = {
		:user_name => ENV['PROTONMAIL_LOGIN'],
		:password => ENV['PROTONMAIL_PWD'],
		:domain => 'protonmail.com', 
		#you can also use google.com
		:address => 'smtp.protonmail.com',
		:port => 587,
		:authentication => :plain,
		:enable_starttls_auto => true  
	}

==> test en console avec adresse en @yopmail.com

==> $ heroku config:set PROTONMAIL_PWD='mot_de_passe'

==> test en console avec adresse en @yopmail.com

#### Validation des tests!!!!!




























