## $ gem install rubocop OU gem 'rubocop', '~> 0.57.2' dans Gemfile

## Lancer rubocop

==> se mettre dans le dossier cible

==> $ rubocop

## Restriction sévérité

==> $ touch .rubocop.yml == créer un ficiher dans l'app 

==> dedans

	inherit_from:
	  - http://relaxed.ruby.style/rubocop.yml

	AllCops:
	 DisplayStyleGuide: true
	 DisplayCopNames: true
	 Exclude:
	  - 'db/schema.rb'
	  - 'vendor/**/*'
	  - 'config/environments/*.rb'
	  - 'bin/*'

	Rails:
	 Enabled: True

	Metrics/BlockLength:
	 Exclude:
	  - 'spec/**/*.rb'
	  - 'Guardfile'
	  - 'vendor/bundle'