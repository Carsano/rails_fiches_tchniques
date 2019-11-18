
# *** Ex de navbar logo à gauche - menu à droite ***


## html

	<div class="container-fluid row">
	  <div class="col-md-1 col-xs-0"></div>
	  # espace à gauche
	  <div class="topnav col-md-10 col-xs-12">
	    <div id="logo">
	      <%= link_to root_path do %>
	      # le logo 
	      <% end %>
	    </div>
	    <div class="links" id="myTopnav">
	      <%if current_user != nil %>
	        <%= link_to "ton_lien", ton_path, :method => :ta_method_si_besoin, data: {confirm: "si besoin"}, class: "right" %>
	      # class right importante
	      <% end %>
	      <%= link_to "tes_autres_liens", class: 'right' %>
	      <a href="javascript:void(0);" class="icon" onclick="myFunction()">
	        <i class="fa fa-bars"></i>
	      </a>
	    </div>
	  </div>
	  <div class="col-md-1 col-xs-0"></div>
	  # espace à droite
	</div>

## css

	/* NAVBAR 
	==================================================*/

	.topnav {
	  overflow: hidden;
	  margin-top: 6em;
	  margin-bottom: 7em;
	}

	.topnav a {
	  float: left;
	  display: block;
	  color: #393939;
	  padding: 0 10px;
	  text-decoration: none;
	  font-size: 17px;
	}

	/* Change the color of links on hover */
	.topnav a:hover {
	  color: black;
	  text-decoration: none;
	}

	/* Remove square around links */
	a {
	  outline: none !important;
	}

	 /*Hide the link that should open and close the topnav on small screens */
	.topnav .icon {
	  display: none;
	}

	/* links size */
	.topnav .right {
	  font-size: 25px;
	  color: #404040;
	  float: right;
	}


	/* Responsive navbar */
	@media (max-width: 770px) {
	  .links.responsive {position: relative;}
	  .links.responsive a.icon {
	    position: absolute;
	    right: 0;
	    top: 0;
	  }
	  .links.responsive a {
	    float: none;
	    display: block;
	    text-align: left;
	    color: #404040;
	    font-size: 17px;
	  }

	  .links {
	    margin-top: 140px;
	  }

	  .links a {display: none;}
	  .links a.icon {
	    float: right;
	    display: block;
	    font-size: 25px;
	  }
	} 

## Js

	function myFunction() {
	  var x = document.getElementById("myTopnav");
	  if (x.className === "links") {
	    x.className += " responsive";
	  } else {
	    x.className = "links";
	  }
	} 