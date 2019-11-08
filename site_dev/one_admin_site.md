## lancer $ rails new -d postgresql name

## Créer la repo github et brancher l'appli

==> git remote add origin ...

==> git add .

==> git commit

==> git push origin master

## $ bundle install

## $ rails db:create

## Intégrer template / kit_ui_bootswatch.md

==> cf template_bootstrap.md/kit_uibootswatch.md

## Créer controller static_pages + router pour l'instant sur home

$ rails controller static_pages home about contact

==> routes.rb

root to: "static_pages#home"
get 'static_pages#contact'
get 'static_pages#about'

## Heroku

Plus vite l'appli est banché en prod, mieux c'est pour tester en live

==> cf heroku.md 

# Tests

## Model + Controller

==> cf mvc.md

==> Model Project...

==> Controller Projects


#Tests

## Log_in 

==> cf gem_bcrypt.md + admin_sessionsimple.md

#Tests


## Intégration des flash bootstrap ou non

==> cf flash_message.md


## Interface admin

==> cf admin_section.md

## Active Storage

==> cf active_storage.md 

==> has_many_attached: simple mosaique avec accès un élément
Difficilement gérable, surtout pour changer une photo....

==> has_one_attached * nombre de fichier lié: plus de souplesse dans l'affichage 

#Tests

## Contact_form

cf contact_form.md



