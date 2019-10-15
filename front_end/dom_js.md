
# *********** DOM et JavaScript **************

	En liant un fichier HTML à un fichier JavaScript, tu vas permettre à ce dernier d'accéder au contenu de la page que tu es en train de coder. Et l'interface qui permet au fichier .js de faire ça s'appelle le Document Object Model ou DOM. Ce DOM est, en définitive, le standard du web pour accéder au contenu HTML d'un fichier.


## console.log(document) == accède à tout le html, <html> == objet

==> var h = document.head;

	console.log(h) == accès à tout le contenu de <head>

==> var b = document.body

	console.log(b) == accès à tout le contenu de <body>


## en cliquant sur document, on tout le html de la page

==> permet de cibler le document

==> permet de cibler des éléments dans ce document


## Sélection des éléments

### var nomElement = document.getElementById('nom_id')

==> console.log(nom_element)

### var nomElement = document.getElementsByTagName('nom_element')

==> console.log(nom_element)

### var nomElement = document.getElementsByClassName('nom_class')

==> console.log(nom_element)

# par soucis de compatibilité, sélection des éléments CSS

### var nomElement = document.querySelector('.class_name') ou ('#id_name')

==> console.log(nomElement)


## Interaction avec document

### nomElement.innerHTML = "Ce qu'on veut"

==> injecte "Ce qu'on veut" dans le nomElement du html

### .className dans la console

==> on peut changer le nom de la class de l'élément sélectionné

	ex: div.className == rend le nom de la class actuelle
			div.className = "nouveau_nom" == change le nom de la class

### .classList == affiche les noms de class de l'éléments

==> on peut rajouter un nom de class: nom_element.classList.add = '....'

	ex: div.classList.add = 'nom_nouvelle_class'

==> on peut supprimer une class: nom_element.classList.remove = '....'

	ex: div.classList.remove = 'nom_nouvelle_class'

==> on peut modifier le style css d'un element: nom_element.style.nom_propriété_css = '...'

	ex: var p = document.getElementById('paragraphe')
			p.style.color = 'red'

			==> met le contenu de l'id 'paragraphe' de l'element p en rouge

# Pour doc supplémentaire

==> www.developer.mozilla.org/

==> devdocs.io

## Résumé

Grâce à ces deux ressources, tu comprends bien qu'il y a 2 étapes quand on veut modifier un élément HTML :

    ==> Le cibler via un selecteur du DOM et le "stocker" dans une variable JS.
    Par exemple : let elementsToBeModified = document.getElementsByTagName('p') pour récupérer tous les éléments paragraphe (tag HTML <p>). elementsToBeModified contiendra plusieurs éléments du DOM, un peu comme un array.

    ==> Passer ensuite une méthode du DOM sur la variable JS pour modifier l'élément qu'elle identifie.
    Par exemple : elementsToBeModified[0].innerHTML = "coucou" pour modifier le contenu HTML du premier élément <p> stocké dans elementsToBeModified.
