
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

https://help.systeme.io/fr/article/comment-mettre-en-place-lauthentification-de-votre-nom-de-domaine-pour-ameliorer-la-deliverabilite-de-vos-emails-1hmvp3n/

https://sendgrid.com/docs/ui/account-and-settings/how-to-set-up-domain-authentication/

## Aller dans Settings/Sender Athentication

==> rentrer les infos pour le dns records

==> rentrrer les infos link branding

==> cliquer sur vérify

## si ok, clique sur le domain de domain authentication

Accès aux DNS records à donner à OVH

# Ce sont les DKIM Records

Le DKIM (Domain Keys Identified Mail) est une technique d’authentification par courriel qui permet au destinataire de vérifier qu’un email a bien été envoyé et autorisé par le propriétaire de ce domaine. Ceci est fait en donnant une signature numérique à l’email. Cette signature DKIM est un en-tête ajouté au message et sécurisé avec un cryptage.

Une fois que le récepteur (ou le système de réception) a déterminé qu’un e-mail est signé avec une signature DKIM valide, il est certain que certaines parties de l’e-mail parmi lesquelles le corps du message et les pièces jointes n’ont pas été modifiées. Habituellement, les signatures DKIM ne sont pas visibles pour les utilisateurs finaux, la validation est effectuée au niveau du serveur.

La mise en œuvre de la norme DKIM améliorera la délivrabilité des courriels et vous protégera contre les courriels malveillants envoyés au nom de votre domaine. Pourtant, dans la pratique ces objectifs sont atteints plus efficacement si vous utilisez le DKIM record avec le DMARC (et même le SPF). Le DMARC et le DMARC Analyzer utilisent à la fois le SPF et le DKIM. Ensemble, ils fournissent une synergie et le meilleur résultat pour la sécurité et la délivrabilité de la messagerie.


	subdomain.yourdomain.com. | CNAME | uXXXXXXX.wlXXX.sendgrid.net
	s1._domainkey.yourdomain.com. | CNAME | s1._domainkey.uXXX.wlXXX.sendgrid.net.
	s2._domainkey.yourdomain.com. | CNAME | s2._domainkey.uXXX.wlXXX.sendgrid.net.


