https://github.com/minimagick/minimagick

## gem 'mini_magick'

## bundle install

## sudo apt-get install imagemagick

## Restart server

## Ex for model Project has_one_attached :image

## require 'mini_magick' in the controller or model where the image is resized

## in model Project

	def image_size
		return self.image.variant(resize: 'sizexsize(!)')
	end

==> on peut appliquer toutes les actions de la gem à image!!!!

ex:

	def image_size
		return self.image.variant(resize: '90x90', colorspace: 'Gray', rotate: '-90')
	end

==> sort une image en '90x90', en noir et blanc, rotation de 90 degrés sens inverse des aiguilles d'une montre!!!! 

## in view 

	<%= image_tag @project.image_size, class: 'img-responsive', alt: 'image'%>

