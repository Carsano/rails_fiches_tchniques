## Gemfile 

gem 'nokogiri'

travaille avec gem 'rubysl-open-uri', '~> 2.0' 
# permet d'ouvrir l'url

$ bundle install

## In file 

require 'nokogiri'
require 'open-uri'

page = Nokogiri::HTML(open("ton_url_a_scrapper.com"))
==> Ouvre l'URL souhaitée sous Nokogiri et le stocker dans un objet page


all_emails_links = page.xpath('/mettre_ici_le_XPath')
==> Extrait les éléments HTML que tu veux scrapper en utilisant leur XPath et en les stockant dans un ARRAY!!!!!

all_emails_links = page.css('/mettre_ici_le_XPath')
==> Extrait les éléments HTML que tu veux scrapper en utilisant leurs classes css et en les stockant dans un ARRAY!!!!!

## Xpath ex 

		page.xpath('//a') == tous les liens d'une page

    page.xpath('//h1') == tous les titres h1

    page.xpath('//h1//a') == liens situés sous un titre h1, lui-même imbriqué dans une div)

    page.xpath('//h1/a') == liens situés DIRECTEMENT sous un titre h1 (sans élément intermédiaire)

    page.xpath('//h1/*') == TOUS les éléments HTML situés DIRECTEMENT sous un titre h1

    page.xpath('//h1[@class="primary"]/a[@id="email"]') == le lien ayant l'id email situé sous un titre h1 de classe primary

    page.xpath('//a[contains(@href, "mailto")]') == tous les liens dont le href contient le mot "mailto"


    all_emails_links.each do |email_link|
      puts email_link.text #ou n'importe quelle autre opération de ton choix ;)
    end 

    == récupérer le texte de chaque lien ? Il faut parcourir l'array et extraire le .text de chaque élément HTML 

	   email_link['href'] == le texte du href d'un élément HTML



PAGE_URL = 'http://ruby.bastardsbook.com/files/hello-webpage.html'

# page = Nokogiri::HTML(open(PAGE_URL))

# puts page.class

# using rest client

page = Nokogiri::HTML(RestClient.get(PAGE_URL))
puts page.class

puts page.css('title')[0].name # => title
puts page.css('title')[0].text # => My webpage

# Set URL to point to where the page exists

links = page.css('a')
puts links.length # => 7
puts links[0].text # => Click here
puts links[0]['href'] # => http://www.google.com

# Limiting selectors

news_links = page.css('a').select { |link| link['data-category'] == 'news' }
news_links.each { |link| puts link['href'] }
# =>   http://reddit.com
# =>   http://www.nytimes.com

puts news_links.class #=> Array

# Select by attribute

news_links = page.css('a[data-category=news]')
news_links.each { |link| puts link['href'] }
#=>   http://reddit.com
#=>   http://www.nytimes.com

puts news_links.class #=> Nokogiri::XML::NodeSet
page.css('p').css('a[data-category=news]').css('strong')
page.css('p a[data-category=news] strong')

# Exercice

new_links = page.css('div#references a')
new_links.each { |link| puts "#{link.text}\t#{link['href']}" }







