# ****** Webfonts google/asset pipeline rails ****

https://fonts.google.com/
https://google-webfonts-helper.herokuapp.com/fonts/

# Dans le cas où la police ne passe en prod

# permet d'intégrer des polices google sans link dans le head et/ou de faire passer en prod dossier fonts dans les templates mis dans lib ou vendor qui ne passeront pas sur heroku....


## Si pas de dossier fonts dans template

### Repérez le ou les noms de la ou des polices utilisées (généralement dans style.css du template)

### aller sur https://google-webfonts-helper.herokuapp.com/fonts/

==> chercher la police

==> select charset

==> select styles

==> copy css: va afficher le @font-face de ce qu'on a choisit

==> download files: téléchargement des fonts

# Télécharger les fonts/extraire/renommer

### Dans l'app

==> copier/coller le dossier font téléchargé dans app/assets/stylesheets

==> créer un fichier un_nom.css.erb dans app/assets/stylesheets

==> dedans, copier/coller le css @font-face généré par https://google-webfonts-helper.herokuapp.com/fonts/

==> modifier le contenu copié/collé

ex: 

de 

	/* quicksand-regular - latin */
	@font-face {
	  font-family: 'Quicksand';
	  font-style: normal;
	  font-weight: 400;
	  src: url('../fonts/quicksand-v19-latin-regular.eot'); /* IE9 Compat Modes */
	  src: local(''),
	       url('../fonts/quicksand-v19-latin-regular.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
	       url('../fonts/quicksand-v19-latin-regular.woff2') format('woff2'), /* Super Modern Browsers */
	       url('../fonts/quicksand-v19-latin-regular.woff') format('woff'), /* Modern Browsers */
	       url('../fonts/quicksand-v19-latin-regular.ttf') format('truetype'), /* Safari, Android, iOS */
	       url('../fonts/quicksand-v19-latin-regular.svg#Quicksand') format('svg'); /* Legacy iOS */
	}

==> intégrer des <%= asset_path ....

==> penseer à mettre la bonne route

==> virer local

modifier en 

	/* quicksand-regular - latin */

	@font-face {
	  font-family: 'Quicksand';
	  font-style: normal;
	  font-weight: 400;
	  src: url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.eot') %>'); 
	  src: url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.eot?#iefix') %>') format('embedded-opentype'), 
	       url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.woff2') %>') format('woff2'), 
	       url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.woff') %>') format('woff'), 
	       url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.ttf') %>') format('truetype'), 
	       url('<%= asset_path('google_webfonts/quicksand-v19-latin-regular.svg#Quicksand') %>') format('svg'); 
	}


## Si dossier fonts dans template

### copier/coller/renommer le ou les dossiers fonts ou les fusionner

### créer un ou plusieurs fichiers nom_fichier.css.erb dans app/assets/stylesheets

### aller sur https://google-webfonts-helper.herokuapp.com/fonts/

### récupérer le @font-face de la police ou de chaque police 

### les intégrer dans les fichiers .css.erb correspondant et les modifier comme décrit plus haut







