
# *************** Atomic Design ***************

https://demos.creative-tim.com/now-ui-kit/index.html 

Les briques de Legos (les atomes) peuvent être assimilées à un kit UI : ce dernier va représenter une application de la charte graphique de ton site sur les éléments basiques de ton application. Ainsi ces éléments seront :

    - Les couleurs
    - La typographie
    - Les boutons
    - Les formulaires
    - Les icônes
    - Et bien d'autres


# 1) Les couleurs

https://webflow.com/blog/web-design-101-color-theory


    - La couleur primary. C'est la couleur maitresse de ton application et tu la retrouveras un peu partout
    - La couleur secondaire. Elle peut être complémentaire à ton application et t'aidera à designer d'autres parties
    - Une palette de gris : gris très léger, gris un peu plus foncé, gris très foncé. On a toujours besoin de gris dans son application
    - Enfin, les couleurs warning, danger, success, info qui permettront de designer tes alertes ou tes boutons

https://color.adobe.com/create
https://coolors.co/
http://colormind.io/

Commence par choisir 5 couleurs :

    ==> La couleur principale (primary)
    ==> La couleur secondaire (secondary)
    ==> Trois nuances de gris :
        - Un gris très foncé (dark)
        - Un gris plus clair (medium-light)
        - Un gris très léger (light)

Il te faudra aussi trouver des couleurs pour les boutons, les alertes :

    - warning dans les tons oranges
    - danger dans les tons rouges
    - success dans les tons vertes
    - info dans les tons bleues

Une fois les couleurs, définies, dans css:


	/************************/
	/* 	    COULEURS        */
	/************************/

	.primary {
		background-color: #000000;
	}

	.text-primary {
	  color: #000000;	
	}


# 2) La typographie

La typographie correspond à tout ce qui est police d'écriture. Comme pour les couleurs, ce sont des détails qui mine de rien changent beaucoup de choses. On ne va pas utiliser une Helvetica Neue comme on va utiliser Avenir. La typographie va correspondre à :


    - Tous les headers (<h1>, <h2>, etc)
    - Le paragraphe (<p>)
    - Les citations <blockquote>
    - Les petits textes (<small>)

Pour chaque élément, il te faudra décider de la taille, du weight, et de la police. Voici quelques conseils :

    - Pour la weight, mets les headers en gras, le reste en normal
    - Les headers ont une police de titre (ex : Montserrat), les paragraphes ont une police lisible (ex : Roboto)
    - 2 polices, c'est largement suffisant
    - Les headers ont une grosse taile (51px pour le h1 par exemple), les paragraphes sont plus petits (14px en général)


https://drive.google.com/file/d/1e18XN6br79cb8kP_v4Qw18k4LBD0nczS/view

https://fonts.google.com/
https://www.typewolf.com/recommendations

Tu devras choisir deux polices pour ton kit :

    - Une police pour les headings (police de titre)
    - Une police pour le texte normal

Puis tu devras designer la typographie des éléments suivants :

    - de <h1> à <h6>
    - les <p>
    - le tag <lead>, qui correspond à un paragraphe à qui tu veux donner plus d'importance
    - les <blockquote>, pour les citations
    - les <ul>, <ol>, et <li> pour les listes


# 3) Les boutons

Les boutons sont un élément central de toute application. Tu devras en faire, et il y a moultes façons de designer un bouton. Pour t'inspirer, va sur tes sites préférés, et regarde comment ces derniers gèrent les boutons. En général tu devras manipuler :

    - La couleur du bouton (background-color)
    - Le radius (à quel point les coins sont arrondis)
    - La couleur de la police du bouton
       => La couleur de ta police doit bien constraster avec celle du bouton si tu veux que cette dernière soit lisible. On te conseille un contraste à partir de 3,5 sur WebAIM
    - Tout ce qui concerne le hover (quand la souris passe dessus)

Tu devras designer les boutons, et afficher les 5 types de boutons :

    - primary
    - warning
    - danger
    - info
    - success


Voici les types de boutons que tu devras faire :

    - Les normaux (btn)
    - Les outline https://getbootstrap.com/docs/4.0/components/buttons/#outline-buttons
    - Les petits (sm) et grand lg

# 4) Les formulaires

Les formulaires sont partie intégrante de ton site. C'est l'endroit où tu vas récupérer l'information. En général, TU vas devoir gérer :

    - Pour les <input> :
       => Le border (épaisseur, taille, couleur)
       => La taille et couleur la typo à l'intérieur, et du placeholder
       => L'arrondi des bords
       => Tout ce qui concerne le focus (quand on a cliqué dessus)
    - Et puis le reste (checkbox, radio button)

https://demos.creative-tim.com/now-ui-kit/docs/1.0/components/forms.html
https://go.webuildthemes.com/components/forms.html
https://htmlstream.com/preview/space-v1.2/documentation/components/forms.html#overview

Tu devras designer les éléments d'un formulaire :

    - Les <input> classiques
    - Les <select>, et les <option> à l'intérieur (le dropdown d'un formulaire)
    - Les <textarea>
    - Les radio buttons
    - Les checkboxes

# 5) Les icônes

Un site va beaucoup utiliser les icônes. Il te faut donc choisir une série d'icônes qui ira bien avec ton design. Voici une liste de sites proposant des icônes :

    FontAwesome, les champions
https://fontawesome.com/

    Boxicons
https://boxicons.com/

    IconicFont
https://iconmonstr.com/iconicfont/

    Batch
http://adamwhitcroft.com/batch/

    Tilda
https://tilda.cc/free-icons/

# 6) Les listes

Tu vas commencer à faire ta première molécules, yay ! La group-list est un élément ressemblant à une carte qui permet d'afficher une liste d'informations utiles.

Une liste est assez simple : tu mets des éléments de texte ensemble, tu ajoutes une image ou une incone ou rien, et puis c'est tout ! Je te laisse designer la liste qui te convient bien. Voici quelques exemples : celui de Bootstrap
https://getbootstrap.com/docs/4.0/components/list-group/#custom-content

celui de Material design
https://material.io/develop/web/components/lists/

ou encore ceux de BootFlat
https://imgur.com/IcN0ccN

Comme tu peux le voir, Bootstrap range les listes non pas dans les ul ou li, mais dans un div avec la classe list-group. En effet, cette liste n'est pas vraiment une liste dans le sens html, mais une liste dans le sens molécule. En gros, la liste-group que l'on a est plus une proto-carte que l'atome ul. Elle te servira à réaliser des cartes pour plus tard.

# 7) Les cartes

Une des molécules les plus importantes d'une application : les cartes. Il existe plein de types de cartes (tu peux en voir ici par exemple) :

    - La carte simple, avec photo, titre, paragraphe, bouton
    - La même carte sans photo
    - Une carte avec photo et paragraphe
    - Une carte avec un icone / avatar à gauche et titre + paragraphe à droite
    - Une carte avec une photo qui prend tout et un titre par dessus

Va sur la doc de Bootstrap, choisis 5 cartes qui iront bien avec ton site, puis designe-les en fonction de ton futur site.











