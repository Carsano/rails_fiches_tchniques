
# Config

==> heroku addons sendgrid

==> ovh email pro

==> ovh dns linked to heroku app (cf ovh_heroku_dns.md)

# OVH

## configurer email pro

https://docs.ovh.com/fr/emails-pro/premiere-configuration/

## configuration automatique des MX

https://docs.ovh.com/fr/domains/mail-mutualise-guide-de-configuration-mx-avec-zone-dns-ovh/

OU 

==> Professionnal emails

==> ton_email

==> associated domains

==> cliquer sur MX dans la ligne

==> confirm


## configuration manuelle

==> Emails

==> ton_email

==> dans General Information

==> Antispam/Anti-virus filtering

==> cliquer sur les trois points

==> choisir

==> va ajouter les MX en fonctions


### si Redirection only choisi vers une autre adresse mail

https://docs.ovh.com/fr/emails/guide-des-redirections-emails/

==> dans General Information/Plan

==> Solution: redirect

==> dans Emails/Emails

==> clique sur Manage redirections

==> add a redirection

# Test

# Autentifier Sendgrid auprès d'ovh

==> pour se connecter au compte sendgrid, utiliser les clé de l'addon dans Reveal config de l'app

https://sendgrid.com/docs/ui/account-and-settings/how-to-set-up-domain-authentication/


