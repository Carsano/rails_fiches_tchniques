# *********** Download file *************

<!-- ## Dans le controller concerné

	  def download_pdf
	    send_file(
	    	"#{Rails.root}/chemin/du/pdf/nom_du_fichier.pdf",
	    	type: "application/pdf"
	    	x_sendfile: true
	    	)  
	  end -->


## Dans la view

Deux solutions/deux types de liens

### Soit le pdf s'affiche sur le navigateur, puis on peut le télécharger ou l'ouvrir après

	<%= link_to "Download Pdf", "/assets/nom_du_fichier.pdf", :class => "themed_text", class: "btn btn-lg btn-custom" %>

<!-- ### Soit on peut cash télécharger le PDF ou l'ouvrir

	<%= link_to "Télécharger", download_pdf_path, :class => "themed_text", class: "btn btn-lg btn-custom" %>


## Dans routes.rb

Il est préférable de mettre en place une route pour l'affichage PDF ou téléchargement direct du PDF

	ex:  get 'download_pdf', to: "homes#download_pdf" -->

