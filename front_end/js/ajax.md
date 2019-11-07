## Principes par rapport au protocole classique


- Une page HTML se charge chez l'utilisateur;

- Celui-ci clique sur un lien (un texte / un bouton / etc.) sur la page afin de voir d'autres informations ou de modifier une donnée;

- Son clic sur un lien de l'app Rails envoie une requête HTTP (= une URL 

=> + une action du genre GET/POST/DELETE/PUT) au serveur et cette requête contient un petit paramètre en plus disant "je suis une requête de type AJAX";

- Le serveur regarde les routes et interprète la requête. Il détermine alors quel contrôleur va traiter la requête et quelle méthode (au sein de ce controller) doit s'exécuter;

- La méthode du contrôleur s'exécute (= elle bosse avec params, fait des appels en BDD via les models, etc.) et stocke les infos de la BDD dans les variables puis balance les variables Ruby obtenues 

=> non pas à une view mais à un fichier JavaScript;

=> Le fichier JavaScript (un fichier .js.erb) récupère les variables Ruby, et va s'exécuter afin de modifier le DOM de la page HTML déjà affichée chez l'utilisateur;

=> A ce moment-là, la page de l'utilisateur se modifie devant ses yeux ébahis et sans rechargement. Des informations sont rajoutées et affichées. Il voit le résultat de son action : l'info de la BDD lui a été transmise.

## Étape 1: travailler sur un lien HTML classique et fonctionnel

## Étape 2: indiquer que le lien va suivre le protocole AJAX

Pour transformer notre lien HTML en un "lien AJAX", il faut rajouter un petit paramètre à ton lien ou à ton formulaire:

==> remote : true

Dans le cas d'un lien simple

	<%= link_to "Déjà Lu", read_path, class: "btn btn-primary", remote: true %>

Dans le cas d'un formulaire form_tag

	<%= form_tag(root_path, :id => 'form_tag_with_remote_true', remote: true) do %>
	 #le code de ton formulaire
	<%= submit_tag 'save', :name => 'commit'%>
	<% end %>

Pour info, ce paramètre remote: true dans tes helpers va (une fois converti en HTML chez l'utilisateur) rajouter l'option data-remote="true" dans ton formulaire ou ton lien et faire disparaître l'authenticity_token. data-remote="true" signifie qu'au lieu de soumettre ton formulaire de manière classique, il sera soumis de manière asynchrone via AJAX.

# Il est possible d'utiliser remote: true avec d'autres helpers de Rails : button_tag, form_for, etc...

# Il existe même un helper de formulaire qui part du principe que tu vas utiliser AJAX et pour lequel remote :true est déjà activé (inutile de le rajouter donc) : c'est form_with.

## Étape 3: permettre au controller de renvoyer vers un fichier JS

il faut rajouter une sorte d'aiguilleur au controller. Cela permettra :

- Qu'il renvoie vers une view html.erb si la requête initiale était "classique" (sans remote: true)

- Qu'il renvoie vers un fichier js.erb si la requête initiale était en AJAX (avec remote: true)

- Qu'il renvoie du JSON dans certains cas (on en parlera pas ici, mais c'est utile quand tu codes une API en Rails)

ex: 

		def create
	    @book = Book.create(book_params)

	    respond_to do |format|
	      format.html { redirect_to books_path }
	      format.js { }
	    end
	  end


## Étape 4: écrire le JavaScript qui va insérer l'info dans la page

Mais je le crée comment ce fichier js.erb?

Il faut te rappeler que :

=> La convention pour nommer ce fichier est "nom de la méthode du controller" + ".js.erb". Quelques exemples : show.js.erb, update.js.erb, create.js.erb, etc.

=> Ce fichier doit être situé dans le même dossier que les views du controller.

C'est à toi de créer ce fichier puis de le coder. 
Tout comme tu peux mettre du Ruby dans tes views, tu pourrais faire des <%=@book.title %> au milieu de ton code JS. 
Tu pourras aussi appeler des partials, passer des variables et combiner cela à la puissance du JavaScript.

### Quelques petits tests préalables

==> mettre alert("le fichier JS est exécuté!"); dans le.js.erb pour tester la prise en compte du fichier

### Le code JS de base 

Globalement, voilà ce que va contenir a minima ton fichier js.erb :

    - Un sélecteur qui va chercher l'élément du DOM à modifier ;

    - Un morceau HTML que le code va rajouter (il est préférable de le mettre sous forme de partial pour ne pas noyer ton code JS avec du HTML).

ex:

	document.getElementById("book-list").insertAdjacentHTML("beforeend", "<li>nouveau livre </li>")

# N'affiche pas le résultat souhaité si create

==> partial

### La partial qui ajoute le bon HTML

==> Créer un fichier app/views/books/_book.html.erb. Dedans, on met le code html.erb qui doit être rajouté en AJAX:

ex: 

	<li>"<%= book.title %>" by <b><%= book.author  %></b></li>

==> Cette partial fait appelle à une variable ruby s'appelant book. On va appeler la partial via create.js.erb en lui passant la bonne valeur de variable. Tout ça grâce au code suivant : <%= render 'book', book: @book %>. Bien sûr, on a déjà le réflexe d'utiliser escape_javascript avant... ou plutôt sa version ultra-raccourcie : j. Voici ce que donne le contenu de create.js.erb 

ex:

	document.getElementById("book-list").insertAdjacentHTML("beforeend", "<%= j render 'book', book: @book %>")




