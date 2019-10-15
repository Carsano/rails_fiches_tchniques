# ************ Services Rails **************

## 1. Introduction

Là tu es en train de te dire : c'est cool Rails, mais comment je fais pour y mettre le code de mon scrapper de l'infini ? Remplir les controller et y mettre des méthodes scrap_mairies n'est pas très clean, donc nous allons voir un truc cool qui s'appelle les services. Les services sont une feature de Rails (pour changer) qui permettent d'y mettre du PORO (Plain Old Ruby Objects), c'est à dire du code Ruby, que tu pourras appeler où tu veux dans Rails. Plutôt cool, non ?

## 2. Historique et contexte

Rails est très MVC. Or, il y a des bouts de code qui font appel à des APIs externes qui n'ont rien à voir avec ton controller ou tes models. Il est donc bien vu de ranger certains codes dans des services qui sont des fichiers spécialement conçus pour ranger le code de ce genre.

## 3. La ressource

Nous allons commencer par un petit tuto fait maison pour que tu fasses ton premier service. Fais une app rails-service, puis crée un dossier services à l'intérieur du dossier app. Dans ce dossier tu peux y mettre tous les services, et les appeler en créant des objets.

Chaque fichier du dossier services invoque une seule class (tu te souviens du tic-tac-toe ? 😉), que tu pourras appeler librement dans ton application rails. Testons. Crée un fichier say_hello.rb dans le dossier services, puis colles-y les lignes suivantes :

	class SayHello
	  def perform
	    p "bonjour"
	  end
	end

Sauvegarde ton fichier, puis lance ta console, ou relance-la si elle est déjà lancée (très important de relancer la console). Dans la console, rentre, SayHello.new.perform. La console devrait t'afficher "bonjour".

Dans le cas où la console te renvoie une erreur genre NameError: uninitialized constant SayHello, il se peut que tu doives relancer spring avec $ bin/spring stop

Voyons-voir ce que cela a fait. Rails a appelé la classe SayHello, a créé une nouvelle instance de la classe, puis lui a demandé d'exécuter la méthode perform. C'est la convention à utiliser pour les services : il faut créer une classe par fichier, qui fait un truc précis (dire bonjour, encaisser un user, chercher un film sur une API), pour pouvoir appeler la méthode perform qui exécute tout ce que la classe doit réaliser.

Tu auras remarqué que l'on a appelé le fichier say_hello.rb et la classe SayHello. C'est (encore) une convention de Rails. Le nom du fichier doit être le snake_case du nom de la classe.

Tu peux appeler tes services où tu veux. Par exemple dans tes controllers, avec la syntaxe SayHello.new.perform. Les services permettent d'avoir du code propre et bien rangé, et de ne pas remplir les controllers.

## 4. Points importants à retenir

Les services se rangent dans le dossier app/services/. Pour les appeler tu peux faire (quasiment partout dans ton app) : TonService.new.perform.

## 5. Pour aller plus loin

    NetGuru ont fait un bon article qui explique les services
    Rob Race a fait aussi un bon article sur les services

