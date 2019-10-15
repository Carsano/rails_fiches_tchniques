

# ************ Stripe *************

## https://stripe.com/docs/checkout/rails

## Envoi de mail après paiement

# ActionMailer après commande

Comment brancher ActionMailer après une commande (Stripe).

## Différence avec un ActionMailer normal == mettre la commande dans le controller charges

Normalement, quand on veut envoyer un mail(sign-in, sign-up,...) on le fait dans le Model, par exemple après la création de quelquechose (after_create). 

###Avec Stripe, pas de Model Charge

==> dans le Controller charges, on va mettre la ligne qui appelle l'envoi de mails dans la méthode create, après la création du paiement:

	class ChargesController < ApplicationController

	  def new
	    @amount = Order.find(params[order_id]).total_price
	  end

	  def create
	    # Amount in cents
	    @order = Order.find(params[:order_id])
	    @amount = (@order.total_price * 100).to_i

	    customer = Stripe::Customer.create({
	                                           email: params[:stripeEmail],
	                                           source: params[:stripeToken],
	                                       })

	    charge = Stripe::Charge.create({
	                                       customer: customer.id,
	                                       amount: @amount,
	                                       description: 'Rails Stripe customer',
	                                       currency: 'usd',
	                                   })
	    @order.update(paid: true)

	 ### UserMailer.deliver_order(current_user, @order).deliver_now ###

	    AdminMailer.notify_admin(current_user, @order).deliver_now
	    flash[:success] = 'Order paid'
	    redirect_to items_path
	    rescue Stripe::CardError => e
	    flash[:error] = e.message
	    redirect_to new_charge_path
	  end
	end

# Mets cette ligne APRES la création du Stripe::Customer et du Stripe::Charge, au cas ou ceux-ci échouent.

On lui passe current_user et la commande correspondante qu'on va utiliser dans la méthode deliver_order


## Passer au user_mailer.rb

Une fois cela fait, on peut passer au user_mailer.rb pour envoyer un mail à l'utilisateur après sa commande.

### N'oublies pas de mettre cette ligne tout en haut dans la classe (comme pour un mail normale): 


	class UserMailer < ApplicationMailer

	  ### default from: 'no-reply@tonsite.fr' ###

	end


### créer une méthode deliver_order, qui prend 2 arguments:

	class UserMailer < ApplicationMailer

	  default from: 'no-reply@tonsite.fr'

	### def deliver_order(user, order)
		    @order = order
		    @user = user 
		    # On pourra utiliser @order et @user dans les views du mail!

		    mail(to: @user.email, subject: 'Ta commande') 
		    # C'est cette ligne-là qui envoie le mail au bon utilisateur, en précisant le subject du mail
			 end
	end


### pour lier un fichier au mail == dans la méthode

==> coder pour sortir le ou les fichiers à envoyer

==> puis ajouter après

	attachments.inline["nom_du_fichier"] = File.read("path/to/nom_du_fichier")
	# pour attacher des fichiers qui existent dans l’app

	attachments.inline["nom_du_fichier"] = File.read(nom_du_fichier.blob.service.send(:path_for, nom_du_fichier.key))
	# pour attacher des fichiers dans la mail qui existent via ActiveStorage


	ex pour site de vente de photos

		class UserMailer < ApplicationMailer
	   default from: 'no-reply@chatons.fr'
	 
		  def welcome_email(user)
		    @user = user 
		    @url  = 'https://chatons-stras-staging.herokuapp.com' 
		    mail(to: @user.email, subject: 'Bienvenue chez nous !') 
		  end
		  # méthode pour envoi de mail à l'inscription

		  def deliver_order(user, order)
		    @jtoi_array = JoinTableOrderItem.where(order_id:order.id).to_a
				# récupération de tous les items relatifs à l'order

		    @jtoi_array = JoinTableOrderItem.where(order_id: order.id).to_a

		    @jtoi_array.each do |o| 
		      @image = if o.item.avatar_attached?
							        o.item.avatar
							      else 
							        o.item.image_url
							     end
		      
		      if o.item.avatar_attached?
		        attachments.inline["#{@image}"]
		        # mets en pièces jointes du mail le fichier ActiveStorage 
		      else
		        attachments.inline["#{@image}"] = File.read("app/assets/images/#{@image}")
		        # mets en pièces jointes du mail le fichier fixé dans l'app
		      end
		    end
		    
		    @user = user 
		    mail(to: @user.email, subject: 'Ta commande')
		    # envoi du mail avec les pièces jointes récupérées 
		  end

	end


## View du mail à envoyer

==> dans le .html.erb

	ex: 

	<p> Salut <%= @user.first_name%>,</p><br> 
	<p>Ta commande a bien été prise en compte!</p>
	<p>Tu trouveras les photos commandées en pièces jointes!</p>

==> dans le .txt.erb 

	ex:

	Salut <%= @user.first_name%>,
	***************************** 
	Ta commande a bien été prise en compte!
	Tu trouveras les photos commandées en pièces jointe

