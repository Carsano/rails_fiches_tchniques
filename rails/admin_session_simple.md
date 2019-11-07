## Avec gem 'bcrypt'

==> cf gem_bcrypt.md

# Une fois un user crée en console

# Redémarrer le serveur!!!

## Session

En Rails, session est un hash qui est stocké dans un coin du navigateur web et qui va suivre le visiteur tout au long de sa navigation. 
Il est grosso modo créé quand ce dernier arrive sur ton site, et est détruit quand il ferme son navigateur. 
À l'inverse de params, qui se vide de son contenu à chaque changement de page, le hash session conserve les informations jusqu'à fermeture du navigateur. 

Ainsi, à chaque nouvelle page chargée, on pourra retrouver quel utilisateur est en train de la visualiser. 
Tu feras ça dans tes controllers avec les lignes suivantes:

	id = session[:user_id]
	@user = User.find(id) #et hop, cette variable @user est l'instance User contenant toutes les infos de l'utilisateur connecté

# Pour gérer les connexions à ton app Rails, on va gérer des "sessions" via un sessions_controller qui contiendra les méthodes avec #new, #create, et #destroy. 

- new == login

- create == authentification 
Si ce dernier est bien authentifié, nous stockerons l'info avec session[:user_id] = user.id. S'il n'est pas bien authentifié, on fera un render de la page de login avec les messages d'erreur.

# Une fois l'utilisateur connecté, ce sera très simple de retrouver notre utilisateur grâce à User.find(session[:user_id])

- destroy == déconnexion

### sessions_controller

#### $ rails g controller sessions new create destroy

Une session est créée à l'instant où un utilisateur se connecte. 
On a alors une information dans le hash session[:user_id]. 
Si personne n'est connecté => session[:user_id] = nil.

==> new + create

	class SessionsController

		def new
			#sert uniquement à avoir une view pour demander le nom et mot de passe
		end

		def create
	  # cherche s'il existe un utilisateur en base avec l’e-mail
	  user = User.find_by(email: email_dans_ton_params)

		  # on vérifie si l'utilisateur existe bien ET si on arrive à l'authentifier (méthode bcrypt) avec le mot de passe 
		  if user && user.authenticate(password_dans_ton_params)
		    session[:user_id] = user.id
		    # redirige où tu veux, avec un flash ou pas

		  else
		    flash.now[:danger] = 'Invalid email/password combination'
		    render 'new'
		  end
		end

	end

==> destroy

Destroy permet de déconnecter l'utilisateur. Cliquer sur le bouton "Se déconnecter" revient à supprimer le contenu de session[:user_id].
Le lien de ton bouton "Se déconnecter" devrait ressembler à ceci:

	<%= link_to "Se déconnecter", session_path(session.id), method: :delete %>

Dans sessions_controller

	def destroy
	  session.delete(:user_id)
	end

### Helpers

==> intro

Nous allons apprendre à changer un User.find_by(id: session[:user_id]) en un plus simple current_user avec deux raisons en tête :

    En Rails, c'est le mal de faire un appel au model (= une requête SQL) dans une vue. C'est le rôle du controller ! Le moindre find dans la view et c'est -30 points en test technique.
    En Rails on aime le code bien DRY et bien lisible. current_user c'est très lisible et plus court que l'autre pâté.

Pour refactorer (= condenser) de cette façon notre code, nous allons passer par un helper. Les helpers se rangent dans le dossier app/helpers/ : ils ont pour mission de créer des méthodes pour remplacer les bouts de code qu'on utilise fréquemment. Tout ce que tu as à faire, c'est de mettre dans ton controller : 

	class TonController < ApplicationController
	  include TonHelper
	end

TonHelper correspond au fichier app/helpers/ton_helper.rb qui devrait ressembler à ceci :

	module TonHelper
	  def some_method
	    # une méthode et son code
	  end
	end

Nous allons donc créer un helper pour les sessions des utilisateurs. 

#Toutefois, il faut savoir qu'on va y faire appel dans quasiment TOUS les controllers et se taper un include SessionsHelper dans chacun d'eux, ça va vite être fastidieux. La solution, c'est de rajouter la ligne dans app/controllers/application_controller.rb qui est, en quelque sorte, le parent de tous les controllers.

Donc une fois la ligne rajoutée, il faut créer un fichier app/helpers/sessions_helper.rb et y mettre les lignes suivantes :

	module SessionsHelper
	   def current_user
	    User.find_by(id: session[:user_id])
	  end

	   def log_in(user)
	    session[:user_id] = user.id
	  end

	end

### before_action

Prenons l'exemple d'une page qui ne peut être accessible QUE par un utilisateur connecté (par exemple une page en /dashboard). Comment mettre en place un filtre pour rejeter les personnes non connectées ? Ça c'est une mission pour le controller ! Eh bien c'est exactement le taf du controller ! Ce dernier va vérifier si l'utilisateur est login, et puis il va le rediriger vers la page de login s'il n'est pas connecté.

On pourrait imaginer de faire quelque chose comme ça dans la méthode qui pointe vers /dashboard:

	def index
	unless current_user
	  flash[:danger] = "Please log in."
	  redirect_to new_session_path
	else
	  # on code quelque chose qui permet d'afficher le dashboard de l'utilisateur
	end

Sauf qu'on peut faire ça beaucoup plus proprement. La bonne façon de le présenter, c'est de faire ça dans une méthode privée authenticate_user, et de l'appeler EN AMONT de la méthode du controller, grâce à un callback before_action. Voici le résultat:

	class TonController < ApplicationController
	  before_action :authenticate_user, only: [:index]

	  def index
	    # on code quelque chose qui permet d'afficher le dashboard de l'utilisateur
	  end

	  private

	  def authenticate_user
	    unless current_user
	      flash[:danger] = "Please log in."
	      redirect_to new_session_path
	    end
	  end

	end

