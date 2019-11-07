# Trois formats très cools pour le stockage de données :

    - le format JSON, très répandu sur le Web

    - les fichier Google Spreadsheet, pour t'aider à t'automatiser des tâches en conjugant Ruby et Spreadsheet

    - le format CSV, très connu aussi pour les données

## JSON 

https://www.w3schools.com/js/js_json_intro.asp

https://stackoverflow.com/questions/5507512/how-to-write-to-a-json-file-in-the-correct-format/5507535#5507535

https://www.tutorialspoint.com/json/json_ruby_example.htm


==> appli qui sort des données sous formes d'un Hash

	def json_method

		tempHash = perform
		#hash de sortie de l'appli

		File.open("db/emails.json","w") do |f|
							#loca du fichier json, mode écriture "w"
		  f.write(JSON.pretty_generate(tempHash))
		  # écriture de chaque ligne du tempHash dans le .json
		end
	end


## CSV == CSV a l'avantage d'être intégré à ruby!!!!

https://www.rubyguides.com/2018/10/parse-csv-ruby/

https://ruby-doc.org/stdlib-2.0.0/libdoc/csv/rdoc/CSV.html

==> conditions idem == données dans un Hash

	def csv_method
		tempHash = perform
		#hash de sortie de l'appli

		File.open("db/emails.csv", "w") { |f| f.write tempHash }
		#loca du fichier json, mode écriture "w"
		# écriture de chaque ligne du tempHash dans le .csv
	end


## Google spreadsheet

### gem 'google_drive'

https://github.com/gimite/google-drive-ruby

### bundle install

### necessite API google!

https://github.com/gimite/google-drive-ruby/blob/master/doc/authorization.md

