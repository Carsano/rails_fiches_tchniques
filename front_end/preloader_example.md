# ***** Ex de preloader avec nom et quatre points qui chargent en dessous ****

## html

	<div id="preloader">
	  <div class="pre-container">
	    <div>
	      # nom ou logo
	    </div>
	    <div class="lds-ellipsis"><div></div><div></div><div></div><div></div></div>
	    # 4 points qui chargent
	  </div>
	</div>

## css

preloader et pre-container == affiche la page de chargement

	#preloader {
	    position: fixed;
	    top: 0;
	    left: 0;
	    right: 0;
	    bottom: 0;
	    background-color: #f5f5f5;
	    z-index: 1000;
	}

	.pre-container {
	    position: absolute;
	    left: 50%;
	    top: 50%;
	    bottom: auto;
	    right: auto;
	    -webkit-transform: translateX(-50%) translateY(-50%);
	    transform: translateX(-50%) translateY(-50%);
	    text-align: center;
		}

le reste == animation 4 points

	.lds-ellipsis {
	  display: inline-block;
	  position: relative;
	  width: 80px;
	  height: 80px;
	}
	.lds-ellipsis div {
	  position: absolute;
	  top: 33px;
	  width: 13px;
	  height: 13px;
	  border-radius: 50%;
	  background: #595959;
	  animation-timing-function: cubic-bezier(0, 1, 1, 0);
	}
	.lds-ellipsis div:nth-child(1) {
	  left: 8px;
	  animation: lds-ellipsis1 0.6s infinite;
	}
	.lds-ellipsis div:nth-child(2) {
	  left: 8px;
	  animation: lds-ellipsis2 0.6s infinite;
	}
	.lds-ellipsis div:nth-child(3) {
	  left: 32px;
	  animation: lds-ellipsis2 0.6s infinite;
	}
	.lds-ellipsis div:nth-child(4) {
	  left: 56px;
	  animation: lds-ellipsis3 0.6s infinite;
	}
	@keyframes lds-ellipsis1 {
	  0% {
	    transform: scale(0);
	  }
	  100% {
	    transform: scale(1);
	  }
	}
	@keyframes lds-ellipsis3 {
	  0% {
	    transform: scale(1);
	  }
	  100% {
	    transform: scale(0);
	  }
	}
	@keyframes lds-ellipsis2 {
	  0% {
	    transform: translate(0, 0);
	  }
	  100% {
	    transform: translate(24px, 0);
	  }
	}


# Preloader spinning + logo

Idée préloader: fond de couleur + logo fixe + trois traits de couleurs vitesse spin !=

fin du chargement: logo + traits

## Créer une partial _preloader.html.erb

## Ajouter

	<div id="loader-wrapper">
	# partie qui recouvrira tout l'écran pour afficher le preloader

    <div id="loader"></div>
    # sera l'élément animé du preloader
    
    <%= image_tag 'y_logo', id: 'spin-logo'%>
    # ajout du logo

    <div class="loader-section"></div>
    # fond du loader

	</div>


## Dans application.css

		/* fixe la page du preloader */
	#loader-wrapper {
	  position: fixed;
	  top: 0;
	  left: 0;
	  width: 100%;
	  height: 100%;
	  z-index: 1000;
	}

	/* size and position du logo */
	#spin-logo {
		display: block;
	  position: relative;
		left: 58%;
	  top: 40%;
		width: 80px;
		height: 80px;
		margin: -75px 0 0 -75px;
	  z-index: 1500;
	  /*animation: spin 1s linear infinite;*/
	}


	/* size, position, set la div pour avoir trois carrés de différentes couleur 
	#loader sert de carré de référence */

	#loader {
	  display: block;
	  position: relative;
	  left: 54.5%;
	  top: 46.5%;
	  width: 200px;
	  height: 200px;
	  margin: -75px 0 0 -75px;
	  border: 3px solid #3498db;
	  z-index: 1500;
	}

	#loader:before {
	    content: "";
	    position: absolute;
	    top: 5px;
	    left: 5px;
	    right: 5px;
	    bottom: 5px;
	    border: 3px solid #e74c3c;
	}

	#loader:after {
	    content: "";
	    position: absolute;
	    top: 15px;
	    left: 15px;
	    right: 15px;
	    bottom: 15px;
	    border: 3px solid #f9c922;
	}

	/* on garde le bord du haut de chaque carré */
	#loader {
	    border: 3px solid transparent;
	    border-top-color: #3498db;
	}
	#loader:before {
	    border: 3px solid transparent;
	    border-top-color: #e74c3c;
	}
	#loader:after {
	    border: 3px solid transparent;
	    border-top-color: #f9c922;
	}


	/* on arrondit les bords */
	#loader {
	    border-radius: 50%;
	}
	#loader:before {
	    border-radius: 50%;
	}
	#loader:after {
	    border-radius: 50%;
	}

	/* include this only once */
	@-webkit-keyframes spin {
	    0%   {
	        -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
	        -ms-transform: rotate(0deg);  /* IE 9 */
	        transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
	    }
	    100% {
	        -webkit-transform: rotate(360deg);  /* Chromehttps://projects.lukehaas.me/css-loaders/, Opera 15+, Safari 3.1+ */
	        -ms-transform: rotate(360deg);  /* IE 9 */
	        transform: rotate(360deg);  /* Firefox 16+, IE 10+, Opera */
	    }
	}

	@keyframes spin {
	    0%   {
	        -webkit-transform: rotate(0deg);  /* Chrome, Opera 15+, Safari 3.1+ */
	        -ms-transform: rotate(0deg);  /* IE 9 */
	        transform: rotate(0deg);  /* Firefox 16+, IE 10+, Opera */
	    }
	    100% {
	        -webkit-transform: rotate(360deg);  /* Chrome, Opera 15+, Safari 3.1+ */
	        -ms-transform: rotate(360deg);  /* IE 9 */
	        transform: rotate(360deg);  /* Firefox 16+, IE 10+, Opera */
	    }
	}


	/* Speed of spinning  des trois traits */
	#loader{
	    -webkit-animation: spin 2s linear infinite;
	    animation: spin 2s linear infinite;
	}
	#loader:before {
	    -webkit-animation: spin 3s linear infinite;
	    animation: spin 3s linear infinite;
	}

	#loader:after{
		-webkit-animation: spin 1.5s linear infinite;
		animation: spin 1.5s linear infinite;
	}

	/* fond du loader
	=============================================*/

	#loader-wrapper .loader-section {
	    position: fixed;
	    top: 0;
	    width: 100%;
	    height: 100%;
	    background: white;
	    z-index: 1000;
	}

	#loader {
	    z-index: 1001; 
	    /* anything higher than z-index: 1000 of .loader-section */
	}

	/* disabled the js */
	.no-js #loader-wrapper {
	    display: none;
	}

## Dans application.js

	$(window).load(function() {
	    $("#loader-wrapper").fadeOut("fast");
	});
