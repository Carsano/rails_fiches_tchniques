# ****** Flash Message Rails ******

## Rails prévoit 4 types de flash

==> notice, pour les informations

==> success, pour les opérations fructueuses

==> alert, pour annoncer une information importante

==> error, pour annoncer une erreur

## Définir un flash


### Idée de base non stylé 

==> mettre le flash dans la méthode du controller souhaité

	flash[:ton_type] = "Ton message !"

==> dans la view layouts/application.html.erb

	<%= flash[:ton_type] %>

==> C'est tout!


## Idée stylé == avec Bootstrap

### Bootstrap n'utilise pas les mêmes termes des 4 types de flash

==> dans app/helper/application_helper.rb

	def bootstrap_class_for_flash(type)
	  case type
	    when 'notice' then "alert-info"
	    when 'success' then "alert-success"
	    when 'error' then "alert-danger"
	    when 'alert' then "alert-warning"
	  end
	end

# Les flash s'utilisent un peu comme les partials

### Dans views/layouts, crée un fichier _flash.html.erb

==> contiendra le message à afficher

	<% flash.each do |type, msg| %>
	# pour chaque flash, affiche

	  <div class="alert <%= bootstrap_class_for_flash(type) %> alert-dismissable show" role="alert">
		# donne le type de flash

	    <%= msg %>
	    # le message du flash

	  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
	    <span aria-hidden="true">×</span>
	  </button>
	  </div>
	<% end %>


### Dans layouts/application.html.erb 

	<%= render 'layouts/flash'%>

==> entre le header et le contenu de la page


### Dans la méthode du controller concerné

Dans ton controller, tu peux facilement créer des flash en faisant flash[:ton_type] = "Ton message". Comme tu as codé des models robustes avec plein de validations, tu as envie de pouvoir appeler les erreurs sans trop te prendre la tête. C'est possible en faisant @user.errors ! Ainsi, si tu veux pouvoir créer un flash et lui donner les erreurs de ton model, tu peux écrire : flash[:error] = @user.errors.full_messages.to_sentence.

par ex:

	def create
	  @post = Post.new(post_params)
	  if @post.save
	    flash[:success] = "Post was successfully created."
	    redirect_to @post
	  else
	    flash.now[:error] = @post.errors.full_messages.to_sentence
	    # voir plus loin
	    render :new
	  end
	end


### Différence flash et flash.now

Lorsque tu n'arrives pas à faire .save, tu fais render :la_view alors que tu fais un redirect_to lorsque tu arrives à faire .save. Pourquoi ? Car en faisant render tu gardes la variable d'instance @user, ce qui est bien pour pouvoir afficher ses erreurs ou encore préremplir le formulaire avec ce que l'utilisateur a mis auparavant. Le soucis est que ton flash va s'afficher pour deux pages puisque tu ne fais pas de redirection. Il te faudra faire un flash.now[:ton_type] = "Ton message".

    Si tu fais un render :some_view, tu fais un flash.now[:ton_type] = "Ton message"

    Si tu fais redirect_to, tu fais un flash[:ton_type] = "Ton message"