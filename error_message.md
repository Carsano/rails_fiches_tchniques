

## Affichage des erreurs

==> dans le controller

	flash[:error] = @variable_controller_name.errors.full_messages.to_sentence

## ActionController::UnknownFormat in controller_name#method

	controller_name#method is missing a template for this request format and variant. request.formats: ["text/html"] request.variant: []

==> pb de redirection dans la method du controller mentionné