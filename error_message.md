# En local

## Affichage des erreurs

==> dans le controller

	flash[:error] = @variable_controller_name.errors.full_messages.to_sentence


## ActionController::UnknownFormat in controller_name#method

	controller_name#method is missing a template for this request format and variant. request.formats: ["text/html"] request.variant: []

==> pb de redirection dans la method du controller mentionné


# HEROKU

## Error: Invalid CSS after

==> généralement un pb de syntaxe dans les fichier css du template

==> meilleur moyen: virer tous les require et les rajouter un à un pour voir où ça merde!

## Error: Invalid CSS after "... filter: progid": expected "}", was ": DXImageTransform."

pour (ex)

	filter: progid: DXImageTransform.Microsoft.gradient( startColorstr='#f3c642', endColorstr='#ee6a50', GradientType=1);

==> faire une recherche de dximage

==> les remplacer par 

	unquote("progid:DXImageTransform.Microsoft.gradient(enabled=false)");

OU par (ex) == enlever espace entre : et D

	filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f3c642', endColorstr='#ee6a50', GradientType=1);

OU

Les supprimer