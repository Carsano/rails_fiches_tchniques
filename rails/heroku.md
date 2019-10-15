## En prod (Heroku)

# Mettre dans environments/develepment config.action_mailer.default_url_options = { :host => 'YOURAPPNAME.herokuapp.com' }

### AVOIR COMMITÉ!!!!

### $ git push heroku master

### $ heroku run rails db:create

### $ heroku run rails db:migrate

### +/- $ heroku run rails db:seed

# ATTENTION, POUR RENDRE LES IMAGES DANS HEROKU:

==> dans config/environments/production.rb 

Mettre "config.assets.compile = false" en "true"