
# ****** Utilisation template Bootstrap Rails ****** 
## Trouver un template 

==> site de bootstrap
==> https://themewagon.com/theme_tag/free/

## rails new -d postgresql app_name

## Asset pipeline == gère tout le css et js présent dans les dossiers assets

Il y en a trois!

==> app/assets: La partie permettant de stocker le css qu'on écrit à la main. c'est à dire si l'on veut un partial pour écrire du css spécifique à une view, c'est ici qu'on le créera.

==> lib/assets: Ici on met nos librairie perso.

==> vendor/assets: Ici on intègre nos librairie externes, comme Bootstrap par exemple, c'est donc de ce dossier qu'on aura besoin pour notre template.

ATTENTION

# Bien sûr il est possible de faire ce que l'on veut, on peut par exemple tous mettre dans notre dossier app/assets, mais en terme d'organisation ça risque de devenir compliqué. C'est pourquoi le plus intéréssant est d'importer les libraires style boostrap dans vendor, puis les css et js principaux de l'app dans app ou dans lib.

## Télécharger le dossier du template 

## Dans application.html.erb

==> Copier/coller tout le code du fichier index.html présent dans le template bootstrap

==> Attention à bien conserver les lien <%  %> dans le <head>

		<%= csrf_meta_tags %>
	  <%= csp_meta_tag %>

	  <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>

	  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>

==> pour mettre le template en responsive, ajouter dans le <head> 

 <meta name="viewport" content= "width=device-width, initial-scale=1.0"> 


## En fonction de ce qu'il y a dans ce dossier

==> copier/coller les dossiers css et js du lib/assets du template (s'il y en a) dans le lib/assets de l'appli

	├── lib
	  │   └── assets
	  │       ├── css [venant du dossier d'origine du template]
	  │       └── js [venant du dossier d'origine du template]

==> copier/coller les dossiers de vendor/assets du template (s'il y en a) dans le vendor/assets de l'appli

	└── vendor
	      └── assets
	          └── [Intégralité des dossiers assets/vendor du template]

On peut avoir au final

	  ├── app
	  ├── bin
	  ├── config
	  ├── db
	  ├── lib
	  │   └── assets
	  │       ├── css [venant du dossier d'origine du template]
	  │       └── js [venant du dossier d'origine du template]
	  ├── log
	  ├── public
	  ├── storage
	  ├── test
	  ├── tmp
	  └── vendor
	      └── assets
	          └── [Intégralité des dossiers assets/vendor du template]

## Appel des fichiers css et js du template dans rails

De base ton app rails va s'initialiser en checkant uniquement le app/assets, il faut donc lui faire comprendre que tu veux qu'elle recherche aussi dans vendor et lib.

### Dans config/initializers/assets.rb, mettre:

==> Rails.application.config.assets.paths << Rails.root.join('lib')

==> Rails.application.config.assets.paths << Rails.root.join('vendor')

### Pour appeler les fichiers:

==> ouvrir le index.html du template 
==> clique droit, View Page Source

#### pour le css 

==> repérer les fichiers css utilisés par le template (dans le <head>)

	ex: <link rel="stylesheet" href="css/bootstrap.css">
	    <link rel="stylesheet" href="css/animate.css">
	    <link rel="stylesheet" href="css/owl.carousel.min.css">
	    <link rel="stylesheet" href="css/magnific-popup.css">
	    <link rel="stylesheet" href="fonts/ionicons/css/ionicons.min.css">
	    <link rel="stylesheet" href="fonts/fontawesome/css/font-awesome.min.css">
	    <link rel="stylesheet" href="fonts/flaticons/font/flaticon.css">

==> dans application.css, copier/coller la partie href 

	/*
	 * This is a manifest file that'll be compiled into application.css, which will include all the files
	 * listed below.
	 *
	 * Any CSS and SCSS file within this directory, lib/assets/stylesheets, or any plugin's
	 * vendor/assets/stylesheets directory can be referenced here using a relative path.
	 *
	 * You're free to add application-wide styles to this file and they'll appear at the bottom of the
	 * compiled file so the styles you add here take precedence over styles defined in any other CSS/SCSS
	 * files in this directory. Styles in this file should be added after the last require_* statement.
	 * It is generally better to create a new file per style scope.
	 *
	 *= require_tree .
	 *= require_self

	 *= require Dossier_de_ta_librairie/Ton_fichier

	 css/bootstrap.css
	 css/animate.css

	 etc

	 */

==> mettre devant *= require assets/ et enlever le .css
A priori, ces fichiers sont dans les assets de lib et/ou vendor


	 *= require assets/css/bootstrap
	 *= require assets/css/animate

### faire de même pour le js

==> repérer les fichiers js utilisés par le template

	ex: <script src="js/jquery-3.2.1.min.js"></script>
	    <script src="js/popper.min.js"></script>
	    <script src="js/bootstrap.min.js"></script>
	    <script src="js/owl.carousel.min.js"></script>
	    <script src="js/jquery.waypoints.min.js"></script>

	    <script src="js/jquery.magnific-popup.min.js"></script>
	    <script src="js/magnific-popup-options.js"></script>

==> dans application.js, copier/coller la partie src + //= require Dossier_de_ta_librairie/Ton_fichier

	/ This is a manifest file that'll be compiled into application.js, which will include all the files
	// listed below.
	//
	// Any JavaScript/Coffee file within this directory, lib/assets/javascripts, or any plugin's
	// vendor/assets/javascripts directory can be referenced here using a relative path.
	//
	// It's not advisable to add code directly here, but if you do, it'll appear at the bottom of the
	// compiled file. JavaScript code in this file should be added after the last require_* statement.
	//
	// Read Sprockets README (https://github.com/rails/sprockets#sprockets-directives) for details
	// about supported directives.
	//
	//= require rails-ujs
	//= require activestorage
	//= require turbolinks
	//= require_tree .

	//= require Dossier_de_ta_librairie/Ton_fichier

	js/bootstrap.min.js
	js/owl.carousel.min.js

==> mettre devant //= require assets/ et enlever le .js

	//= require assets/js/bootstrap.min
	//= require assets/js/owl.carousel.min

# ATTENTION!!!!

# Lorsque tu appelles des ressources se trouvant dans les sous répertoires du dossier lib de ton app, il ne faut pas ajouter le sous repertoire, mais uniquement le nom du fichier que tu souhaites appeler.

# Attention à ne pas TOUT appeler non plus, les fonts et les icons fonctionnent différemment du reste et ne sont pas récupérés de la même façon.

## Il ne reste plus qu'à remplacer le texte du template par le texte de l'appli!!!

## La plupart des templates sont fournis avec des pages d'exemples. Très pratique pour commencer le développement avec une base. N'hésite pas à choisir une page, la copier totalement et broder autour. C'est beaucoup plus facile pour commencer.







