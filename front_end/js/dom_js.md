
# *********** DOM et JavaScript **************

	En liant un fichier HTML à un fichier JavaScript, tu vas permettre à ce dernier d'accéder au contenu de la page que tu es en train de coder. Et l'interface qui permet au fichier .js de faire ça s'appelle le Document Object Model ou DOM. Ce DOM est, en définitive, le standard du web pour accéder au contenu HTML d'un fichier.


## console.log(document) == accède à tout le html, document == <html> == objet

==> permet de pouvoir cibler le document

==> puis cibler les éléments que l'on veut !

ex:

	var h = document.head;
	console.log(h) == accès à tout le contenu de <head>

	var b = document.body;
	console.log(b) == accès à tout le contenu de <body>

	var f = document.forms ==> sort toutes les balises form
	var f = document.divs ==> sort toutes les balises div


# Sélectionner un élément

## Accéder aux éléments

### getElementBy

#### document.getElementById('nom_id')

ex: 

	var nomElement = document.getElementById('id_name');
	console.log(nom_element);

	==> affiche le ou les éléments ayant comme id="id_name"


#### getElementsByTagName('nom_element')

ex: 

	var nomElement = document.getElementsByTagName('div')
	console.log(nom_element)

	==> affiche le ou les éléments ayant comme balise div


#### getElementsByClassName('nom_class')

ex: 

	var nomElement = document.getElementsByClassName('class_name')
	console.log(nom_element)

	==> affiche le ou les éléments ayant comme nom de class="class_name"


### querySelector ou querySelectorAll

==> permet d'utiliser les sélecteurs CSS

==> . pour les classes, # pour les id

#### document.querySelector('.class_name') == renvoit le premier élément

#### document.querySelectorAll('#id_name') == renvoit tous les éléments


## Accéder au contenu

### nomElement.innerHTML 

==> affiche tout le contenu html de l'élément 


### nomElement.textContent 

==> renvoie le contenu textuel, sans éventuel balisage


## Accéder au attributs

### nomElement.getAttribute("") 

==> renvoit la valeur de l'attribut (ex href pour les liens)

### nomElement.hasAttribute?("") 

==> pour vérifier si l'élément possède un attribut

### nomElement.attribute

==> renvoie la valeur de l'attribut de l'élément sélectionné

ex:

	nomElement.src


## Accéder aux classes de l'élément

### .className

==> affiche (en console) le nom de la class de nomElement

==> pour changer le nom

	ex: nomElement.className == rend le nom de la class actuelle
			nomElement.className = "nouveau_nom" == change le nom de la class


### .classList == si l'élément a plusiers class, affiche les noms de class de l'élément

==> .classList.add = '....'

permet d'ajouter un nom à la class: 

ex: 

	div.classList.add = 'nom_nouvelle_class'
	div.classList == sort la liste des nom de class + la nouvelle ajoutée

==> .classList.remove = '....'

permet de supprimer une class

ex: 

	div.classList.remove = 'nom_class'

### .classList.contains("") == teste la présence d'une classe dans l'élément

### nom_element.style.nom_propriété_css = '...'

==> on peut modifier le style css d'un element: 

	ex: var p = document.getElementById('paragraphe')
			p.style.color = 'red';

			==> met le contenu de l'id 'paragraphe' de l'element p en rouge

			p.style.fontSize = '2em';


# Modifier cet élément 

## changer le html content

==> nomElement.innerHTML = "new_text"

ex:

	nomElement.innerHTML = "new_html"

## changer/ajouter l'attribut

==> nomElement.attribute = new value

ex: 

	nomElement.src = "new_value"

OU 

==> nomElement.setAttribute

ex:

	nomElement.setAttribute("id", "titre")
	// donne id="titre" à l'élément


## Ajouter un nouvel élément

	var pythonElt = document.createElement("li"); 
	// Création d'un élément li

	pythonElt.id = "python"; 
	// Définition de son identifiant

	pythonElt.textContent = "Python"; 
	// Définition de son contenu textuel

	document.getElementById("langages").appendChild(pythonElt); 
	// Insertion du nouvel élément

OU 

	var rubyElt = document.createElement("li"); 
	// Création d'un élément li

	rubyElt.id = "ruby"; 
	// Définition de son identifiant

	rubyElt.appendChild(document.createTextNode("Ruby")); 
	// Définition de son contenu textuel

	document.getElementById("langages").appendChild(rubyElt); // Insertion du nouvel élément


# Les noeuds == node

## Principe

html => head => meta 
		 |			 |
		 |			 |> title
		 |
		 |> body => h1 => titre
		 				 |
		 				 |> p => paragraphe
		 				 |
		 				 |> p => paragraphe
		 				 			|
		 				 			|> a ==> lien 
		 				 			|
		 				 			|> .

html est le root node 
html n'a pas de parents
html est le parent de head et body 
head est le first child de html 
body est le last child de html 
head a deux children: meta et title 
title a un child de type .text_node 
body a 3 children: h1, p, p
h1 a un child de type .text_node: 'titre'
h1 et p sont siblings

On peut naviguer entre les nodes en utilisant les propriétés suivantes:

- parentNode
- childNodes[nodenumber]
- firstChild
- lastChild
- nextSibling
- previousSibling


## Accéder au noeuds

### .childNodes == accéder aux noeuds enfants du noeud élément sélectionné

Il s'agit d'une collection ordonnée regroupant tous ses nœuds enfants sous la forme d'objets DOM. On peut utiliser cette collection un peu comme un tableau pour accéder aux différents enfants d'un nœud

# IMPORTANT: les espaces entre les balises ainsi que les retours à la ligne dans le code HTML sont considérés par le navigateur comme des nœuds textuels.

Pour parcourir la liste des noeuds enfants

	// Affiche les noeuds enfant du noeud body
	for (var i = 0; i < document.body.childNodes.length; i++) {
	    console.log(document.body.childNodes[i]);
	}


#### .parentNode == accéder au parent d'un noeud

ex:

	var h1 = document.body.childNodes[1];
	console.log(h1.parentNode); // Affiche le noeud body

	console.log(document.parentNode); // Affiche null : document n'a aucun noeud parent


https://developer.mozilla.org/fr/docs/Web/API/Node

#### .firstChild == .childNodes.first == .childNodes[0]

#### .lastChild == .childNodes.last == .childNodes[-1]


## Type/valeur du noeud

### .nodeType == indique le type de node 

==> .ELEMENT_NODE == noeud 'éléments'

Ceux qui correspondent à des éléments HTML, comme body, h1 ou p. 
Ces nœuds peuvent avoir des sous-nœuds, appelés fils ou enfants(children).

==> .TEXT_NODE ==  noeud textuel

Ceux qui correspondent au contenu textuel de la page comme 'titre' ou 'paragraphe'. 
Ces nœuds ne peuvent pas avoir de fils.


### .nodeValue == valeur du noeud

==> null pour l'element_node 

==> le text pour un noeud text_node 

==> valuer de l'attribut pour un noeud attribut


## Modif

### Créer un noeud textuel

	var rubyElt = document.createElement("li"); 
	// Création d'un élément li

	rubyElt.id = "ruby"; 
	// Définition de son identifiant

	rubyElt.appendChild(document.createTextNode("Ruby")); 
	// Définition de son contenu textuel

	document.getElementById("langages").appendChild(rubyElt); // Insertion du nouvel élément

### Ajouter un noeud avant un autre

	var perlElt = document.createElement("li"); 
	// Création d'un élément li

	perlElt.id = "perl"; 
	// Définition de son identifiant

	perlElt.textContent = "Perl"; 
	// Définition de son contenu textuel

	// Ajout du nouvel élément avant l'identifiant identifié par "php"
	document.getElementById("langages").insertBefore(perlElt, 
	    document.getElementById("php"));

### Remplacer un noeud

	var bashElt = document.createElement("li"); 
	// Création d'un élément li

	bashElt.id = "bash"; 
	// Définition de son identifiant

	bashElt.textContent = "Bash"; 
	// Définition de son contenu textuel

	// Remplacement de l'élément identifié par "perl" par le nouvel élément
	document.getElementById("langages").replaceChild(bashElt, document.getElementById("perl"));

### Supprimer un noeud

	// Suppression de l'élément identifié par "bash"
	document.getElementById("langages").removeChild(document.getElementById("bash"));



# Pour doc supplémentaire

==> www.developer.mozilla.org/

==> devdocs.io

## Résumé


==> Le cibler via un selecteur du DOM et le "stocker" dans une variable JS.

    let elementsToBeModified = document.getElementsByTagName('p') pour récupérer tous les éléments paragraphe (tag HTML <p>). 

    elementsToBeModified contiendra plusieurs éléments du DOM, un peu comme un array.

==> Passer ensuite une méthode du DOM sur la variable JS pour modifier l'élément qu'elle identifie.

    elementsToBeModified[0].innerHTML = "coucou" pour modifier le contenu HTML du premier élément <p> stocké dans elementsToBeModified.
