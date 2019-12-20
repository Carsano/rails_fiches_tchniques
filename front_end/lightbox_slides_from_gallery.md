#****** Ouverture d'images dans un slide *****

https://ashleydw.github.io/lightbox/#image-gallery

## Install

==> mieux vaut télécharger les fichiers depuis la repo github dans vendor

- le .css
- le .js
- le .min.js

https://github.com/ashleydw/lightbox/tree/master/dist

## Dans application.js

$(document).on('click', '[data-toggle="lightbox"]', function(event) {
    event.preventDefault();
    $(this).ekkoLightbox();
});

## Dans la views

==> mettre data-toggle

	<a href="https://unsplash.it/1200/768.jpg?image=251" data-toggle="lightbox">
	  <img src="https://unsplash.it/600.jpg?image=251" class="img-fluid">
	</a>


==> en ruby + data-gallery="my-gallery"

	<%= link_to url_for(picture), data: {toggle: 'lightbox', gallery:'example-gallery'} do%>
	  <span class="bg"></span>
	  <%= image_tag(picture, class: "img-fluid") %>
	<% end %>


