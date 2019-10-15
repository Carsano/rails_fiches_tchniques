# ------------- Les partials --------------

Les partials permettent de mettre des bouts de code de front dans un fichier à part.

## Si le code vient d'un kit UI, le brancher sur ton app!!!!

## Mettre le code que tu veux appeler dans un fichier _ta_partial.html.erb

# Généralement un partial == une molécule ou un organisme

## Appelle cette "partial" dans ta view ton_fichier.html.erb en utilisant <%= render "ta_partial" %>

## Où mettre les partials

    Si la partial est dans le même dossier que la view, tu l'appelles en faisant <%= render "ta_partial" %>

    Si la partial n'est pas dans le même dossier que la view, tu précises le dossier. Ainsi si je voulais appeler la partial app/views/contacts/_form.html.erb dans la view edit de la ressource users, tu écrirais dans <%= render "contacts/form" %>

###Il est préférable de ranger les partials dans le bon dossier.

    Une partial que l'on appelle depuis le fichier application.html.erb (header, footer, alert) se met dans le dossier app/views/layouts/

    Une partial que l'on appelle dans plusieurs pages de l'application (une bannière) se mettra dans un dossier app/views/shared/

    Une partial qui concerne une ressource précise (une liste, un formulaire, un affichage précis) se met dans le dossier de la ressource. Ainsi, le formulaire pour créer un événement serait un fichier app/views/events/_forms.html.erb

###application.html.erb

Il existe un fichier qui s'appelle app/views/layouts/application.html.erb qui contient les fondements du fichier html de ton application. Le head, et le body. Tu peux y mettre des CDNs facilement, et notamment le fondement d'une page web.

Certaines parties de ton application se retrouvent sur toutes les pages :

    Le header
    Le footer
    Les alertes

Ainsi, nous te conseillons de faire une partial pour chacun de ces éléments (que tu mettras dans app/views/layouts/), et les mettre dans le body du fichier application.html.erb. Cela devrait ressembler à ceci :

	<%= render 'layouts/header' %>
	<%= render 'layouts/flash'%>
	<%= yield %>
	<%= render 'layouts/footer'%>

#Pour ce dossier uniquement, on spécifie 'layouts' même si le partial est dans le même dossier!!!!

## Opération de partials

CHAUD..........





