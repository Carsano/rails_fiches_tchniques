# ************* LINUX **************

## LAMP install

https://bitnami.com/stack/lamp/installer

### run appli install

### install library libtinfo-to-5 si absente

$ sudo apt-get install libncurses5

### db password

user: 'root'
password: 'azerty'

### Wordpress to LAMP

https://wordpress.org/support/article/how-to-install-wordpress/

https://docs.bitnami.com/installer/infrastructure/lamp/

## Télécharger wordpress

https://fr.wordpress.org/download/

unzip


# A faire pour chaque site

## Créer une database via phpMyAdmin



==> Click sur Databases

==> create new database == entrez le nom de l'appli surlaquelle on taff + "utf8 general"

==> retourner sur la page d'accueil

==> aller dans Users Accounts: sélectionner ou créer un nouveau user 

==> créer un nouveau user par appli 

    - Click Add user.

    - Choose a username for WordPress (‘wordpress‘ is good) and enter it in the User name field. (Be sure Use text field: is selected from the dropdown.)

    - Choose a secure password (ideally containing a combination of upper- and lower-case letters, numbers, and symbols), and enter it in the Password field. (Be sure Use text field: is selected from the dropdown.) Re-enter the password in the Re-typefield.

    - Write down the username and password you chose.

    - Leave all options under Global privileges at their defaults.

    - Click Go.
    # Return to the Users screen and click the Edit privileges icon on the user you’ve just created for WordPress.
    # In the Database-specific privileges section, select the database you’ve just created for WordPress under the Add privileges to the following database dropdown, and click Go.
    # The page will refresh with privileges for that database. Click Check All to select all privileges, and click Go.
    # On the resulting page, make note of the host name listed after Server: at the top of the page. (This will usually be localhost.)


==> user edit privileges et le connecter à la database

==> select all privilages sans grant

## Copier/coller le dossier wordpress unzipé dans apache2/htdocs et le renommer avec le même nom de la database créée

## Aller sur localhost:8080/nom_dossier et remplir les données du user créé

## Accès à wp-admin de l'appli

==> ex: http://localhost:8080/nom_dossier/wp-admin/

test_user
azerty

==> créer utilisateur et password pour le dev (user supplementaire client est créé plus tard)

admin
azerty

== uti et password pour accéder à l'espace wordpress !!!!!


## Liens utiles

Site fait (theme astra, plugins astra starter)
https://www.youtube.com/watch?v=8AZ8GqW5iak




