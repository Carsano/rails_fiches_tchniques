# Préparation

## Créer la repo Github

## $ rails new -d postgresql app-name

## Brancher app-name avec repo github

==> git remote add origin ...

==> git add .

==> git commit

==> git push origin master

## $ bundle install

## $ rails db:create


# Créer controller static_pages + router pour l'instant sur home

$ rails controller static_pages home about contact

==> routes.rb

root to: "static_pages#home"
get 'static_pages#contact'
get 'static_pages#about'


# Intégrer template / kit_ui_bootswatch.md

==> cf template_bootstrap.md/kit_uibootswatch.md


# Heroku

Plus vite l'appli est banché en staging et prod, mieux c'est pour tester en live

==> cf heroku.md 

# Tests


# Interface admin 

## Log-in 

==> cf rails/gems/gem_bcrypt.md

## Sessions 

==> rails/admin_section_simple.md

# Tests + Heroku


# Active Storage

## cf active_storage.md 

## has_many_attached: simple mosaique avec accès un élément

==> surtout pour images en vrac avec pas de réel position

==> Difficilement gérable, surtout pour changer une photo....

## has_one_attached * nombre de fichier lié: plus de souplesse dans l'affichage et le remplacement 

# Tests + Heroku


# Model + Controller hors admin

==> cf mvc.md

==> Model Project...

==> Controller Projects

# Tests + Heroku


# Intégration des flash bootstrap ou non

==> cf flash_message.md


# Contact_form

## cf contact_form.md

# Tests + Heroku



