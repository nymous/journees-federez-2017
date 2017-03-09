Gozmail
=======

>alarig@gozmail.net  
>http://alarig.swordarmor.fr/

## Pourquoi être décentralisé/indépendant niveau mail ?

- Ne pas passer par 15 personnes pour changer un truc
- Savoir quand le gouvernement demande accès aux données
- Ne pas se faire sniffer le réseau par les admins réseaux (serveur géré par des gens connus)
- Recevoir les abuse@
- Honnêteté : mail breton, pas de serveurs frontaux à Paris

## Implication

Juridiquement :
  - Faire les paperasses, création d'assso, faire les comptes

Techniquement :

  - Gros SPOF, plus de la moitié de l'infra est chez Grifon
  - Ça coûte cher niveau machine (beaucoup plus cher que chez Online/OVH)

## Se battre contre les gros silos du mail

Ne pas se faire rejeter par les gros (Microsoft, Orange, Gmail) -> se battre pour ça -> avoir une grosse complexité (DKIM, SPF, DMARC...)

**Note** chercher "Mailop mailing list", et "mailop microsoft" pour voir une ML et savoir comment gueuler sur MS pour qu'ils acceptent les mails ^^

## Choix techniques

  - Logiciels libres
  - Logiciels facilement utilisables
  - Logiciels facilement administrables

- OS : debian
- SMTP : postfix (et spamassassin, et opendkim, et mailman, et ...)
- IMAP : dovecot (il gère même les filtres avec sieve)(sirius c'est chiant à configurer)
- Webmail : roundcube

En plus ils supportent IPv6, DNSSEC, poussent au chiffrement

**Note** Munin pour faire des graphes/monitoring


Gmail : ils droppent tous les mails en IPv6  
Orange : ils limitent à 3 mails/s, sinon ça drop

`tinc` : système de VPN, ils envoient les backup entre serveurs en chiffrant avec ce tunnel VPN

`apticron` : envoie des mails quand des majs sont dispo

Les filtres ne sont pas gérés par roundcube, il faut une petite extension pour gérer les filtres sieve
Sinon y'a `sieve-connect` pour les "coder" à la main

Ils utilisent `postfixadmin`

Une doc très très bien faite pour dovecot/postfix sur "workaround"  
Y'a aussi de la doc sur FedeRez

Antispam : rspam en entrée (pour faire des greylists), spamassassin en sortie


Recipe ansible pour Let's Encrypt, écrite par ???????

Choper une sonde RIPE


