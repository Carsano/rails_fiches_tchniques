# ********** fonts icomoon ************

https://icomoon.io/
https://gist.github.com/anotheruiguy/7379570

http://www.openmindedinnovations.com/blogs/custom-font-icons-for-ruby-on-rails-with-icomoon

## cliquer sur IcomoonApp

## sélectionner ces icons en cliquant/décliquant

## Generate Font

## +/- Preferences

## Download

## Déziper le paquet

## Dans le paquet

==> copier/coller le dossier fonts dans app/assets/stylesheets

==> copier/coller le style.css dans app/assets/stylesheets

==> renommer style.css en style.css.erb ou lui donner un autre nom genre icomoon.css.erb

## dans app/assets/stylesheets/style.css.erb

==> changer

	@font-face {
	  font-family: 'icomoon';
	  src:  url('fonts/icomoon.eot?nl4wm8');
	  src:  url('fonts/icomoon.eot?nl4wm8#iefix') format('embedded-opentype'),
	    url('fonts/icomoon.ttf?nl4wm8') format('truetype'),
	    url('fonts/icomoon.woff?nl4wm8') format('woff'),
	    url('fonts/icomoon.svg?nl4wm8#icomoon') format('svg');
	  font-weight: normal;
	  font-style: normal;
	  font-display: block;
	}

==> en

	@font-face {
	  font-family: 'icomoon';
	  src:  url('<%= asset_path('fonts/icomoon.eot?nl4wm8') %>');
	  src:  url('<%= asset_path('fonts/icomoon.eot?nl4wm8#iefix') %>') format('embedded-opentype'),
	    url('<%= asset_path('fonts/icomoon.ttf?nl4wm8')%>') format('truetype'),
	    url('<%= asset_path('fonts/icomoon.woff?nl4wm8')%>') format('woff'),
	    url('<%= asset_path('fonts/icomoon.svg?nl4wm8#icomoon')%>') format('svg');
	  font-weight: normal;
	  font-style: normal;
	  font-display: block;
	}


## Views

## Pour appeler le font dans une view à partir du css

exemple:

	.panel-heading a.collapsed:after {
		font-family: "icomoon";
	  content: "\ea3e"; }


## Mettre le font direct dans la view

ex:

	<span class="icon-arrow-up2"></span>

==> pour changer le css

dans app/assets/stylesheets/style.css.erb

	[class^="icon-"], [class*=" icon-"] {
	  /* use !important to prevent issues with browser extensions that change fonts */
	  font-family: 'icomoon' !important;
	  speak: none;
	  font-style: normal;
	  font-weight: normal;
	  font-variant: normal;
	  text-transform: none;
	  line-height: 1;
	  # mettre le css qu'on veut !!!
	  color: green;
	  ...
	  ...
	  ...

	  /* Better Font Rendering =========== */
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	}