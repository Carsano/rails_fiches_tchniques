## gem 'bcrypt' dans Gemfile

## $ bundle install

## Création model User ou ajout colonne pour le mot de passe

==> $ rails g model User name:string password_digest:string

OU 

==> $ rails g migration AddColumnPasswordToUser

	def change
	  add_column :users, :password_digest, :string
	end

## Mettre has_secure_password dans User

	class User < ApplicationRecord
	  has_secure_password
	end

## Actions possibles

- my_user = User.create(password:"foobar") == un user va être créé et bcrypt transformera "foobar" en hash sécurisé qui sera stocké dans password_digest.

- my_user.authenticate("test de mot de passe")

=> Si ce mot de passe ne correspond pas à celui entré par l'utilisateur, bcrypt te retourne false

=> S’il correspond, bcrypt te retourne tout simplement l'objet my_user.

- Enfin, tu peux faire comme près de 100 % des sites sur Terre et demander, à l'inscription, un password et sa confirmation. Dans ce cas-là, tu peux faire User.create(password: "motdepasse", password_confirmation: "laconfirmation") et bcrypt n'enregistrera un utilisateur en base QUE si password et password_confirmation sont identiques. Sinon Rollback.

# Pour la prod ==> créer l'admin!!!!

==> heroku run rails console

User.create(name:"", password:"")

==> faire un seed.rb qui créé l'admin