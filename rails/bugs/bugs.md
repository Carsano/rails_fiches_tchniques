## Rails Server ne s'arrête pas 

pour l'arrêter
=> ctrl-C ou ctrl-Z

s'il tourne en boucle ou qu'il ne se lance pas parce qu'il tourne en fond

=> $ lsof -wni tcp:3000
prendre le PID

=> $ kill -9 PID

## test appli prod en local

$ export RAILS_ENV=production

Revenir en development

$ export RAILS_ENV=development

## pb git reset --hard SHA

Si veut pas pusher

==> $ git push origin HEAD --force

