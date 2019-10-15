
## librairies avant git

	$ sudo apt-get install autoconf automake bison build-essential curl git-core libapr1 libaprutil1 libc6-dev libltdl-dev libsqlite3-0 libsqlite3-dev libssl-dev libtool libxml2-dev libxslt-dev libxslt1-dev libyaml-dev ncurses-dev nodejs openssl sqlite3 zlib1g zlib1g-dev libreadline7

## git

Normalement installé

	$ git --version

Sinon

	$ sudo apt-get install git


## RVM == Ruby Version Manager

### Installation de RVM

	$ curl -L get.rvm.io | bash -s stable

### Si pb de signature

	$ gpg (ou gpg2) --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

### Si encore pb

==> installer gpg

### Pour vérifier que RVM est bien installée

	$ type rvm | head -1

	$ rvm -v

### Pour que RVM marche convenablement, il faut bien dire à ton terminal en mode "login shell". 


## Ruby (sur la RVM)

$ rvm install 2.5.1

Si RVM te sort une erreur type autoreconf was not found in the PATH, lance la commande $ sudo apt-get install automake puis relance $ rvm install 2.5.1

$ ruby -v

## Rails

$ gem install rails -v 5.2.3

$ rails -v

## Configurer github

# Afin d'éviter tout problème de correspondance, il faut veiller à utiliser la même adresse email pour GitHub, Heroku, Git et SSH.

$ git config --global user.name "Ecris ton nom ici, en gardant les quotes"

$ git config --global user.email "Ecris ton email ici, en gardant les quotes"

Pour vérifier que git a bien pris en compte tes modifications lance la commande $ git config --get user.name puis $ git config --get user.email. Si les résultats correspondent à ce que tu as indiqué avec les commandes précédentes tout est bon !

Nous allons désormais configurer GitHub en SSH. 

Avant de commencer, vérifie que tu ne possèdes pas déjà une clé SSH, avec la commande $ ls ~/.ssh/id_rsa

Si tu n'obtiens pas ce résultat mais plutôt quelque chose comme No such file or directory, ca signifie que tu n'as pas de clé SSH. Pas de souci, nous allons en créer une ! Lance la commande $ ssh-keygen -C ton_email@ton_email.com -t rsa

Votre nouvelle clé SSH publique a été ajoutée à votre .ssh/, vous pouvez l'afficher en faisant $ cat ~/.ssh/id_rsa.pub. Vous pouvez également afficher votre clé privée en faisant $ cat ~/.ssh/id_rsa, ne partagez jamais ce fichier.

Ajoutez maintenant votre clé à votre agent de connexion en faisant $ ssh-add ~/.ssh/id_rsa

Si vous obtenez une erreur du type Could not open a connection to your authentication agent lancez la commande $ eval `ssh-agent`.

Nous allons maintenant lier votre compte Github et vos clés SSH. Pour commencer, lancez la commande :

    si vous êtes sur macOS : $ pbcopy < ~/.ssh/id_rsa.pub
    si vous êtes sur Linux : $ sudo apt-get install xclip puis xclip -sel clip < ~/.ssh/id_rsa.pub

Cette commande ajoutera dans votre clipboard votre clé SSH, vous pourrez ainsi coller celle-ci lors de la prochaine étape.

Pour configurer Github avec votre clé SSH, vous aurez besoin de vous rendre dans les paramètres de votre profil. Une fois fait, sélectionnez SSH and GPG Keys puis New SSH key, donnez un nom à votre clé puis collez votre clé SSH préalablement copiée avec la commande ci-dessus dans la zone prévue à cet effet. Cliquez ensuite sur Add SSH key. Confirmez en entrant votre mot de passe Github.

Pour confirmer l'authentification SSH lancez la commande $ ssh -T git@github.com dans votre terminal :

## Heroku

Heroku est le service que nous utiliserons pour publier nos applications sur le Web. Posséder un compte et préparer la configuration requise est donc très important !
Pour commencer, enregistrez-vous sur ce lien en utilisant la même adresse mail que vous avez utilisée pour configurer SSH et votre compte github. Validez votre compté et authentifiez-vous avant de passer à la suite.

Installez le client Heroku en suivant la doc disponible sur ce lien. Vérifiez que le CLI a été bien installé en utilisant la commande $ heroku version.

$ heroku version
heroku /7.18.9 darwin-x64 node-v11.1.0

Ajoutez votre clé SSH à Heroku en exécutant la commande suivante $ heroku keys:add. Il vous sera probablement demandé de rentrer vos identifiants heroku, remplissez donc les champs demandés et voilà. Heroku est configuré en SSH !