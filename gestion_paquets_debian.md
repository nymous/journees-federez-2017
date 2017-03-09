Gestion personnalisée de paquets Debian
=======================================

AAAAAAAAAAAAAAAAAH C'EST TROP BIEN
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAH C'EST MÉGA TROP BIEN LE PLAN PUTAIN

## Introduction rapide

Debian c'est quoi ?
Distrib linux avec :
  
  - Système d'installation + gestionnaire de paquets
  - Paquets binaire pour de nombreuses architecture (debian.org/ports)
  - Version stable tous les deux ans environ (debian.org/releases)

Projet :

  - Contributeurs tous bénévoles (mais ils acceptent les dons)
  - Contrat social et principes du logiciel libre (DFSG)(debian.org/social_contract)
  - Assurance qualité : charte Debian (debian.org/doc/debian-policy)

Cyril Brulebois
Développeur Debian
Responsable de publication du Debian Installer (le setup au début)

Consultant logiciel libre (Debamax) (debamax.com)
Membre de l'assocation CapLibre (caplibre.fr)

## Paquets, dépôt, comment ça marche ?

Deux formats : `.deb` et `.rpm`

Dans un paquet :

  - Méta-données : nom, version, dépendances, conflits, description...
  - Contenu : des répertoires, des fichiers (pas que des sources)
  - Scripts: : exécutés à l'installation, la mise à jour, la suppression...

Gestionnaire de paquets

  - Outls bas niveau
    - `.deb` -> `dpkg`
    - `.rpm` -> `rpm`
  Outils haut niveau
    - `apt`, `aptitude`...

Inspections un paquet : `dpkg --info package.deb`
Extraire un fichier : `dpkg --control package.deb`
Contenu du paquet : `dpkg -- contents package.deb`
Extraction sans installation : `dpkg --extract package.deb dest/`

La charte Debian **oblige** à avoir de la doc et surtout un changelog

On peut installer manuellement avec `dpkg -i`, mais il ne gère pas les dépendances donc il peut laisser un paquet à moitié installé s'il n'a pas les dépendances -> `apt-get` (qui gère les dépendances de manière récursives) !

On peut consulter le cache de `apt` :

  - `apt-cache search <package>`
  - `apt-cache show <package>`
  - `apt showpkg <pkg>`

`apt` se gère avec un fichier `sources.list`
(conf minimale : `deb http://ftp.fr.debian.org/debian jessie main`)
(conf plus complète : 
```
deb     http://ftp.fr.debian.org/debian jessie main contrib non-free
deb-src http://ftp.fr.debian.org/debian jessie main

deb     http://ftp.fr.debian.org/debian jessie-backports main
deb-src http://ftp.fr.debian.org/debian jessie-backports main

deb     http://security.debian.org/debian jessie/updates
deb-src http://security.debian.org/debian jessie/updates
```
)

Mise à jour du cache : `apt-get upadte`
On récupère un fichier `Release.gpg` pour vérifier que ça a bien été publié par des gens en qui on a confiance

Conf de apt :

  - `apt.conf(.d)` : conf générale
  - `sources.list(.d)` : dépôts
  - `trusted.gpg(.d)` : clés de confiance (cf `apt-key`)

## Paquets personnalisés

### Pour quoi faire ?

  - Version trop vieille sur les dépôts debian
  - Corriger un bug, ajouter une fonctionnalité
  - Compiler avec d'autres options
  - Créer un nouveau paquet pour soi

Préréquis/recommandations :

  - `dpkg-dev` : commandes `dpkg-source`, `dpkg-buildpackage`
  - `build-essential` : compilateur, `make`
  - `devscripts` : outils pour les dev
  - `lintian` : vérifications automatisés pour paquets Debian **très utile !!!**
  - `sources.list` : lignes `deb-src`

### Exemple

Récupération d'un paquet source : `apt-get source <pkg>` (si on a la ligne `deb-src`)
Un paquet source = un `dsc` (liste de l'ensemble des fichiers contenus dans le paquet), un `tar` (les fichiers) et un `debian/diff` (les modifs faites par un mainteneur debian)

  1. Installer les dépendances de compilation d'un paquet : `apt-get build-dep <pkg>`
  2. Créer une nouvelle entrée dans `debian/changelog` : `dch --local +<pseudo>`
  3. Faire le bugfix/modif
  4. Compiler un nouveau paquet binaire : `dpkg-buildpackage -b` (pour n'avoir que le paquet binaire)
  5. Installer cette nouvelle version : `sudo debi --upgrade`

Pour préparer un paquet source : `dpkg-buildpackage -S`

Problème de tout ça :

  - On ne corrige pas un bug en prod quand même, on n'installe pas toutes les merdes de compil sur la prod -> compiler ailleurs et déployer ailleurs
  - Problèmes de dépendances/versions
  - Architectures différentes
  - Installation des paquets avec résolution de dépendances

### Outils de développement et de déploiement d'app

Solution 1 : VM (yolo, un peu overkill)
Solution 2 : `chroot` pour créer un système minimal dans un sous-répertoire, dans lequel on installe les outils de compilation sans conséquences sur le système hôte

Exemple :
```bash
sudo debootstrap squeeze /scratch/squeeze http://archive.dbian.org/debian
sudo chroot /scratch/squeeze
# on ajoute les lignes deb-src
apt-get update
apt-get source apach"2
cd apache2-*/
apt-get build-dep apache2
dpkg-buildpackage -b
```

Bof, chiant parce que tout est manuel et faut être root

Solution 3 : `schroot`, tout pareil qu'avant mais on a de la conf pour autoriser des paramètres, des points de montage, des réglages réseau, des utilisateurs autorisés...

Ex :
```bash
# /etc/schroot/chroot.d/sid-i386-devel
[sid-i386-devel]
type=directory
description=Debian sid/i386
directory=/home/kibi/chroots/sid-i386
groups=root,kibi
root-groups=root,kibi
profile=default
personality=linux32
```

C'est vachement cool, mais y'a encore mieux !

Solution 4 : `sbuild` qui s'appuie sur `schroot`, mais automatise encore plus
  - `sbuild-createchroot` (`debootstrap` + configuration `schroot`)
  - `sbuild-update`
  - `sbuild` (lancement de compilation dans un chroot donné, installation automatique des build-depends, et nettoyage à la fin)
  - c'est l'outil utilisé sur le réseau de build daemons de Debian

Solution alternative : `pbuilder`
Solution alternative : `cowbuilder`

## Gestion du déploiement : création d'un dépôt

Solutions :

### `apt-ftparchive`

Remplaçant de `dpkg-scanpackages` et `dpkg-scansources`
Pas de `dist/` et de `pool/`, au contraire d'un dépôt normal -> syntaxe particulière pour sources.list
Manuel et lent

### `reprepro`

Gère des dépôts de taille importante (genre debian)
Fait des manipulation par distro, architecture, paquet source...
Suivi fait via une BDD
Peut synchroniser depuis un dépôt distant, de filtrer les paquets...
Création d'instantanés (snapshot) pour figer un environnement de production

Crée la bonne structure de dépôt, met à jour les indices `Packages`, le fichier `Release` (et le signe)...

## Vers l'intégration continue

Croiser deux mondes :

  - Base solide de dépôt : `reprepo`
  - Intégration continue : `jenkins`

-> `jenkins-debian-glue` (jenkins-debian-glue.org)

Inconvénient : un dépôt par paquet plutôt qu'un dépôt global
-> Solution : on modifie le job `reprepo` pour n'en avor qu'un seul, et dans tous les job `binaries` on pointe vers cet unique `reprepo`
