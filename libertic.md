Libertic
========


https://libertic.wordpress.com/

Ouverture des données publiques des villes de moins de 35k habitants

Ils proposent de faire des weekends ouverture de données (sur WP, sur OSM) avec des petites villes (- 3000hab)

Ils ont organisé un truc avec la Poste, pour avoir un retour d'exp sur l'open data

## Open Data & OSS : même combat

Récemment, il y a eu une ouverture de la base de données des adresses postales (avant la Poste avait un bout, l'IGN en avait un autre, y'avait des trous...)

Il y a des règles à respecter et des licences

## Historique

1942 : Robert King a dit qu'il faudrait qu'on arrête de parler de propriété intellectuelle sur les recherches, les découvertes scientifiques, pour que tout le monde puisse en profiter

1978 : fondation de la CNIL et la CADA (Commission d'Accès au Documents Administratifs). La CADA dit que tout producteur de données doit la fournir aux citoyens qui en font la demande

1995 : 1è apparition du terme Open Data

2007 : À Sébastopol en Californie, des penseurs du web ont défini les critères qui définissent l'open data -> 8 critères

2007 aussi : Open Government Act, Washington DC est la première ville à ouvrir ses donnes

2009 : Qqn en économie dit que la data est un bien commun

2010 : Rennes est la 1è ville fr à ouvrir ses données

2011 : Ouverture de l'etalab (data.gouv.fr) pour regrouper les données

2016 : Loi Lemaire qui dit beaucoup de choses 

  - Standards ouverts
  - Réutilisable librement et gratuitement
  - Exploitable facilement par un système automatique
  - On ne peut plus faire payer pour accéder aux données (ex : l'INSEE ne peut plus faire payer pour accéder à la bdd de SIRET)
  - Les données d'intérêt général sont par défaut de l'Open Data
    - Problème : les décisions de justice doivent être ouvertes, mais il faut anonymiser les données
    - Les algos doivent être ouverts
    - Les financement publics doivent être ouverts

Un observatoire en Angleterre nous classe 10è (mais on devrait remonter).

## Technique

Exploitable par process automatique + formats ouverts et libres -> CSV \o/ (PDF, DOC = caca)

## Juridique

Licences ouverte (Open licence) ou ODbl

## Économique

Peu ou pas de redevance tarifaire

Les données sont une matière première, elles sont une condition pour innover facilement

## Limitations

Données numériques NON nominatives

## Quels domaines sont concernés ?

  - Transport
  - Équipements publics
  - Urbanisme (trotoirs surbaissés pour fauteils roulants)
  - Population, entreprises, emploi
  - Résultats élection, budget
  - Qualité de l'air/eau, inventaire des arbres/environnement
  - Équipements culturels

## Qu'est-ce qu'on en fait ?

- Consultation (data.gouv.fr)
- Médiation (comment donner du sens à la donnée) -> étude de Libération sur comment est réparti le budget de l'État
- Applications : ex V³ Predict', disponibilité prévisionnelle (H +12h) des vlib à Bordeaux à partir des data sur 4 ans (à partir de l'historique des stations, de la météo et du jour de la semaine)
- Mobilité du Grand Lyon : (j'ai pas suivi)
- SNCF : ils ont collaboré avec OSM pour localiser les gares, les données des TER ont toujours été en open data avec des API... maintenant ils ont fait une API pour tous leurs trucs, mais ils ont peur que Google (ou d'autres) utilisent leurs données pour vendre des billets aux gens, et qu'ils coupent les clients de la SNCF qui se ferait dicter ses politiques tarifaires par Google -> modèle freemium (peu de requêtes -> c'est gratuit, plus -> c'est payant)

La data c'est de la matière première et une ressource stratégique !

## De l'industrie à la distribution de data

Enedis (réparateur de câbles électriques) a les données de conso depuis 1948

En open data, on a accès aux données de conso au niveau de la maille IRIS (20k hab)(plus précis ça devient des données perso, et il faut le consentement de l'utilisateur)

-> ils ont maintenant un 2è métier : fournisseur de données et API, + gestion des autorisations personnelles

On peut aller voir sur leur portail

On en est encore au stade de "on a plein de données, on en fait quoi ?"

## Urban data

Amsterdam a mis en place plein de données :

  - thermographie aérienne (quels toits perdent de la chaleur ?)
  - potentiels solaires (selon l'orientation du toit, combien d'énergie photovoltaïque on récupère ?)
  - déchets (quel potentiel de déchets par quartier, parce que les déchets ça se brule et ça fait de l'énergie)

## Open data privée

RATP Opendatalab
Capteur pioupiou (anémomètre qui propose une API, le capteur est fait comme ça)
Havas media (open data en 2014)

## Comment on ouvre ses données ?

On va voir Libertic ^^

  1. Cataloguer les info publiques collectées et produites
  2. Anticiper et odifier les clauses des contrats publics
  3. Documenter, mettre des métadonnées, précises les fréquences de maj
  4. Formater en trucs ouverts non propriétaires
  5. Standardiser (Neptune, GTFS, DCAT, RDF)
  6. Publier
  7. Animer, montrer aux gens que les données sont dispo, proposer aux gens des concours pour que ça soit référencé, pour qu'on s'en serve...

## La culture des données (open ou pas)

Sensibiliser, impliquer, expérimenter (parce qu'on ne sait pas encore quoi faire, comment le faire, quoi en faire), développer un vrai projet, organiser


