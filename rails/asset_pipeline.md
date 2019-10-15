
# ************** Asset pipeline ***************

https://www.grafikart.fr/tutoriels/asset-pipeline-851

Qu'est ce que l'Asset Pipeline ? Le but de l'Asset Pipeline est d'optimiser l'accès à des ressources (Image, CoffeScript, SASS, JS, CSS, ...). Des choses extérieures à votre code mais dont votre site aura besoin pour fonctionner. En bref, il va vous demander de renseigner où trouver toutes ces ressources, et ensuite vous permettre de les appeler plus simplement dans votre code. Voir même pour certaines, les précompiler pour optimiser le fonctionnement de votre site. Mieux votre assez pipeline est géré à la base, plus vôtre site sera rapide.

# Asset pipeline gère le css et le javascript, mais va bientôt être remplacé par Webpack dans rails v6
==> pour ne pas activer asset pipeline
$ rails new -d postgresql appname --skip-sprockets

L'asset pipeline récupère le js et le css dans app/assets

# $ rails assets:clean assets:precompile
==> une fois css js ok

# Dans config/environments/production.rb

==> mettre config.assets.compile = true