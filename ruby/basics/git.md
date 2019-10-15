

    $ git init == permet d'initialiser un répo git en local

    $ git add nom_fichier == permet d'ajouter nom_fichier à ton futur commit

    $ git add . == ajoute plusieurs fichiers

    $ git commit -m == "message_commit" permet de faire un commit avec pour message message_commit

    $ git status == permet d'afficher l'état de tes fichiers par rapport au commit que tu es en train de réaliser (⚠ commande très pratique, à faire tout le temps)

    $ git diff nom_fichier == permet d'afficher les modifications de nom_fichier entre le dernier commit et le fichier actuel

    $ git log == permet d'afficher l'historique des commits

    $ git checkout SHA == permet de revenir en mode lecture uniquement au commit SHA

    $ git checkout master == permet de revenir au commit actuel

    $ git reset --hard SHA == permet de tout effacer pour revenir au commit SHA

    $ git reset --hard == permet de tout effacer pour revenir au dernier commit

    $ git stash == permet de sauvegarder provisoirement son travail sans le commit

    $ git stash pop == permet de récupérer un travail précédemment sauvegardé avec git stash

    $ git remote -v == permet de voir les différentes remotes de ton repo git

    $ git remote add nom_remote url_remote == permet d'ajouter la remote url_remote à la liste de tes remotes. Cette remote aura le nom nom_remote (par convention, la remote principale où il y a le code s'appelle origin)

    $ git push nom_remote nom_branche == permet de pousser ta branche nom_branche vers ta remote nom_remote

    $ git pull nom_remote nom_branche == permet de récupérer ta branche nom_branche de ta remote nom_remote

    $ git branch == liste des branches en cours

    $ git branch nom_nouvelle_branche == création d'une nouvelle branche

    $ git checkout nom_branche == permet de changer de branche

    $ git merge nom_branche == permet de fusionner la branche nommée avec la branche sur laquelle on est 
