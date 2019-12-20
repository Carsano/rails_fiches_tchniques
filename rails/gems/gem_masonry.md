https://github.com/kristianmandrup/masonry-rails
https://rubygems.org/gems/masonry-rails

## gem 'masonry-rails', '~> 0.2.4' 

## $ bundle install

## application.js

==> //= require masonry/jquery.masonry

OU

==> //= require masonry/masonry.min

## css

	#masonry-container {
	  background: #FFF;
	  padding: 5px;
	  margin-bottom: 20px;
	  border-radius: 5px;
	  clear: both;
	  -webkit-border-radius: 5px;
	     -moz-border-radius: 5px;
	          border-radius: 5px;
	}

	.clearfix:before, .clearfix:after { content: ""; display: table; }
	.clearfix:after { clear: both; }
	.clearfix { zoom: 1; }

	.col1 { width: 80px; }
	.col2 { width: 180px; }
	.col3 { width: 280px; }
	.col4 { width: 380px; }
	.col5 { width: 480px; }

	.col1 img { max-width: 80px; }
	.col2 img { max-width: 180px; }
	.col3 img { max-width: 280px; }
	.col4 img { max-width: 380px; }
	.col5 img { max-width: 480px; } 

## html

	<div id="masonry-container" class="transitions-enabled infinite-scroll clearfix">

	  <div class="box col3">
	    ...
	  </div>

	  <div class="box col1">
	    <p>Donec nec justo eget felis facilisis fermentum. Aliquam porttitor mauris sit amet orci. </p>
	  </div>

	  <div class="box col2">
	    <p>
	      <a href="http://www.flickr.com/photos/nemoorange/3319714470/" title="Whitlow's on Wilson by nemoorange, on Flickr"><img src="http://farm4.static.flickr.com/3008/3319714470_c05a5cb5a8_m.jpg" alt="Whitlow's on Wilson" /></a>
	    </p>
	  </div>
	</div>
