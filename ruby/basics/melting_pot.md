# *** L'authenticity token ***

Une des forces de Rails est que le framework met automatiquement en place des sécurités sans qu'il ne soit nécessaire de s'en soucier. Ainsi, une forme de piratage fréquente sur Internet est l'utilisation de la vulnérabilité CSRF des sites web. En gros, le formulaire HTML de base ne marchera pas sur Rails, car ce n'est pas très secure : comment savoir si la requête émanant de ton ordi a bien été soumise par un formulaire web et pas par un script tournant, sans que tu ne le saches, sur ton ordi ? Pour s'en assurer, Rails demande systématiquement que les requêtes provenant de formulaires soient accompagnées d'un "authenticity token". Ne t'en fais pas, c'est plutôt simple à implémenter : il faut juste rajouter un champ caché dans ton formulaire. Pour cela, ajoute cela à ton formulaire :

<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">

Grâce au type="hidden", cette ligne sera invisible pour l'utilisateur. Par contre, tu peux checker le code source de ta page : le <%= form_authenticity_token %> sera remplacé par un long string qui validera que la requête provient bien d'un formulaire web et que personne n'usurpe ton identité.

# *** Les sessions ***

3.1. Les sessions
3.1.1. Introduction aux sessions

Quand tu vas sur Facebook, que tu te connectes, et que tu navigues sur le site, ce dernier se souvient que tu es connecté bien que tu passes de page en page. Cette magie noire est plutôt simple à mettre en place : Facebook stocke sur ton navigateur une petite information qui indiquera, à chaque chargement de page, que tu es l'utilisateur numéro XXXX. Cette information est souvent stockée sous la forme de cookies : entre aujourd'hui et demain, nous allons t'apprendre à coder toi-même un système de cookies. La première étape est de comprendre les sessions.

En Rails, session est un hash qui est stocké dans un coin du navigateur web et qui va suivre le visiteur tout au long de sa navigation. Il est grosso modo créé quand ce dernier arrive sur ton site, et est détruit quand il ferme son navigateur. À l'inverse de params, qui se vide de son contenu à chaque changement de page, le hash session conserve les informations jusqu'à fermeture du navigateur.
En tant qu'application, tu peux écrire et lire à volonté sur session. Si par exemple tu fais session[:salut] = "Message sympa" dans la méthode d'un controller, tu pourras retrouver cette info dans un autre controller (par exemple avec puts session[:salut]). Mais l'usage le plus courant de session est de stocker l'id de l'utilisateur actuellement connecté. Ainsi, à chaque nouvelle page chargée, on pourra retrouver quel utilisateur est en train de la visualiser. Tu feras ça dans tes controllers avec les lignes suivantes :

id = session[:user_id]
@user = User.find(id) #et hop, cette variable @user est l'instance User contenant toutes les infos de l'utilisateur connecté

Pour gérer les connexions à ton app Rails, on va gérer des "sessions" via un sessions_controller qui contiendra les méthodes avec #new, #create, et #destroy.

    La création d'une session se fera au travers de la page de login où l'on demande mot de passe et email.
    Puis la méthode #create va s'occuper d'essayer d'authentifier l'utilisateur avec le couple email-mot de passe. Si ce dernier est bien authentifié, nous stockerons l'info avec session[:user_id] = user.id. S'il n'est pas bien authentifié, on fera un render de la page de login avec les messages d'erreur.
    Une fois l'utilisateur connecté, ce sera très simple de retrouver notre utilisateur grâce à User.find(session[:user_id])

🚀 ALERTE BONNE ASTUCE

Tu peux t'amuser à voir tes sessions et cookies dans Inspecteur d'éléments -> Application -> Storage -> Cookies -> Localhost:3000. Si tu supprimes le hash session, c'est comme si tu fermais ton navigateur !

De plus, étant donné que les cookies temporaires de session sont automatiquement chiffrés, faire User.find(session[:user_id]) pour récupérer un utilisateur est sécurisé. Ce n'est pas possible pour un hacker d'utiliser les informations de session pour usurper ton identité. Par contre, les cookies permanents sont moins sécurisés : ils sont vulnérables à une attaque au détournement de session, et nous verrons demain comment être prudent en les utilisant.
3.1.2. sessions_controller

Ici, une session n'est pas un objet en soi. Il n'existe pas de model Session ni d'enregistrement dans la base de donnée. En fait, on considère qu'une session est créée à l'instant où un utilisateur se connecte à son compte sur ton site web : on a alors une information dans le hash session[:user_id]. Si personne n'est connecté, il n'y a pas de session et session[:user_id] = nil.

Une session utilisateur pouvant être créée ou supprimée, il semble logique d'en confier la gestion à un controller à part entière. Il s'agira de sessions_controller et il disposera de trois méthodes :

    #new qui correspond à la page de login
    #create qui effectuera l'authentification (traitement des informations saisies dans la page login et sauvegarde de l'info de l'utilisateur connecté dans session)
    #destroy qui correspond au logout

Les routes seront celles habituelles, grâce à un resources :sessions, only: [:new, :create, :destroy].
a) new et create

La view new va juste demander l’e-mail et le mot de passe à l'utilisateur via un formulaire, ce dernier pointant vers la méthode create.

La méthode create ressemblera à ceci :

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

b) utiliser ensuite l'info de session

Grâce au create de sessions_controller, on a pu stocker l'id de l'utilisateur actuellement connecté.
Maintenant, à partir de cet id, tu peux remonter à l'utilisateur pour avoir ses infos. Par exemple tu peux faire une view /profile qui aura comme code :

<h2>Bienvenue <%= User.find_by(id: session[:user_id]).first_name %> !</h2>

Ou alors créer un potin et le lier immédiatement à l'utilisateur connecté (qui est forcément l'auteur), en mettant dans la méthode create de gossips_controller :

def create
  @gossip = Gossip.create(gossip_params)
  @gossip.user = User.find_by(id: session[:user_id])
  if @gossip.save
    flash[:success] = "Potin bien créé !"
    redirect_to root_path
  else
    render :new
  end
end


Cette façon de faire est tout à fait fonctionnelle, mais tu remarques déjà qu'on écrit plusieurs fois User.find_by(id: session[:user_id]). Et c'est pas très DRY de se répéter en Rails !
Nous verrons plus loin comment remplacer ce code par un simple appel à une méthode current_user qui fera la même chose.
c) destroy

Destroy permet de déconnecter l'utilisateur. Cliquer sur le bouton "Se déconnecter" revient à supprimer le contenu de session[:user_id].
Le lien de ton bouton "Se déconnecter" devrait ressembler à ceci :

<%= link_to "Se déconnecter", session_path(session.id), method: :delete %>

Ce qui a pour effet d'appeler la méthode #destroy de ton sessions_controller. Cette méthode devrait ressembler à ceci :

def destroy
  session.delete(:user_id)
end

3.2. Les helpers
3.2.1. Introduction

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

3.2.2. SessionsHelper

Nous allons donc créer un helper pour les sessions des utilisateurs. Toutefois, il faut savoir qu'on va y faire appel dans quasiment TOUS les controllers et se taper un include SessionsHelper dans chacun d'eux, ça va vite être fastidieux. La solution, c'est de rajouter la ligne dans app/controllers/application_controller.rb qui est, en quelque sorte, le parent de tous les controllers.
Si tu es perspicace, tu auras noté que chaque controller commence par la ligne class TonController < ApplicationController : en fait ils héritent tous de ApplicationController ! Au final, c'est un peu le controller ultime : tout ce qui est écrit dedans, tu peux considérer qu'il est écrit dans chaque controller.

Donc une fois la ligne rajoutée, il faut créer un fichier app/helpers/sessions_helper.rb et y mettre les lignes suivantes :

module SessionsHelper
   def current_user
    User.find_by(id: session[:user_id])
  end

   def log_in(user)
    session[:user_id] = user.id
  end

end

Et voilà ! Maintenant tu peux appeler current_user dans n'importe quel controller ou view, et cette méthode te retournera l'instance de User contenant les infos de ton utilisateur connecté.

Tu noteras même qu'on s'est amusés à faire une méthode login(user), qui nous servira à connecter facilement l'utilisateur. Ça permet de l'appeler à la page login mais aussi juste après la création d'un nouvel utilisateur (par exemple).
⚠️ ALERTE ERREUR COMMUNE

Dans ta view, tu pourrais être tenté d'écrire direct des current_user.email. Mais il faut gérer les 2 cas : 1) un utilisateur est connecté et 2) personne n'est connecté. Là, si personne n'est connecté, tu auras un joli undefined method `email' for nil:NilClass.
Pour éviter ça, prévoit de rajouter un if current_user, ou même coder une méthode logged_in? dans le helper. Cette méthode te retournerait true si session[:user_id] est non-nil et false dans le cas contraire.
3.3. Utiliser before_action

Prenons l'exemple d'une page qui ne peut être accessible QUE par un utilisateur connecté (par exemple une page en /dashboard). Comment mettre en place un filtre pour rejeter les personnes non connectées ? Ça c'est une mission pour le controller ! Eh bien c'est exactement le taf du controller ! Ce dernier va vérifier si l'utilisateur est login, et puis il va le rediriger vers la page de login s'il n'est pas connecté.

On pourrait imaginer de faire quelque chose comme ça dans la méthode qui pointe vers /dashboard :

def index
unless current_user
  flash[:danger] = "Please log in."
  redirect_to new_session_path
else
  # on code quelque chose qui permet d'afficher le dashboard de l'utilisateur
end

Sauf qu'on peut faire ça beaucoup plus proprement. La bonne façon de le présenter, c'est de faire ça dans une méthode privée authenticate_user, et de l'appeler EN AMONT de la méthode du controller, grâce à un callback before_action. Voici le résultat :

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



Grâce à ce magnifique callback, à chaque fois que la méthode index de ton controller est appelée, la méthode authenticate_user va être exécutée en amont. Elle filtrera les utilisateurs non connectés pour les rediriger vers la page login : en quelques lignes tu viens de sécuriser ton application !


