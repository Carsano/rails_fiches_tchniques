
# ************** Heroku/OVH ****************

https://stackoverflow.com/questions/36292485/customize-url-with-heroku-and-ovh

# Préparatif

## OVH

==> création du compte + nom de domaine

==> click Domains/All my domains

## Heroku

# Attention, il faut rentrer sa cb!!!

==> sélectionner l'app ou l'app-prod

==> dans settings, cliquer sur Add Domain

==> rentrer le dns du site en www.tonsite.com/fr/...

==> retourne un DNS target

# Connecter OVH avec Heroku

## OVH

==> allez dans OVH/Domaines/ton_domaine/DNS zone

==> Garder les lignes avec les cibles suivantes

- dns106.ovh.net. 

- ns106.ovh.net.

- 1 redirect.ovh.net.

- ressemblant à 217.386.33.2

==> supprimer les autres

==> click sur Add an entry

==> click sur CNAME

==> rentrer www dans sub-domain


## Heroku

==> copier/coller le DNS target

## OVH

==> coller le DNS target donné par Heroku dans Target et rajouter un "." à la fin

==> click sur Next

==> recap, click confirm

# Attente jusqu'à 24h !!!



# Redirection

## OVH

==> Domains => tondomain => redirection

==> Si une ligne a comme type IPv4 - A, la supprimer, c'est celle que l'on va rajouter dans la redirection

==> click Add a redirection

==> step 1/5: Ne rien remplir + next

==> step 2/5: check to a web address + next

==> step 3/5: select with a visible redirection + next

==> step 4/5: select Permanent (301) + rentrer l'adresse de redirection complète https://www.tonsite.com + next

==> step 5/5: check Confirm the overwriting of.... + Confirm


# Si, après le plug entre ovh et heroku message sur l'adresse de l'app Heroku “build something amazing”

==> $ heroku domains:add www.tonsite.com


# Accès

==> nom-app-prod => Settings => Domains => click sur le lien


# Attention, pas de SSL, donc https://www.monsite.com ne marchera pas !!!!
