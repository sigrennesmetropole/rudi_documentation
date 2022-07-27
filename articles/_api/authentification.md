---
order: 2
title: Authentification
---

# Authentification des fournisseurs de données

Les fournisseurs de données pour publier des métadonnées doivent s'authentifier en OAuth2.

Pour cela, il faut :
* Déclarer le fournisseur dans RUDI Portail (µPoviders)
* Déclarer chaque noeud du fournisseur dans RUDI Portail (µPoviders)
* A chaque noeud est associé un utilisateur de type _ROBOT_  (µACL). L'utilisateur possède :

  * un client_id sous la forme d'un UUID V4
  * un client_secret

Le noeud fournisseur peut alors s'authentifier comme suit :

```
curl -v --request POST http://rudi.bzh/oauth/token --data "grant_type=password" --data "username=username>" --data "password=password>" --data "scope=liste des scopes séparés par des virgules>" --data "client_id=client_id>" -H "Authorization:Basic encodage en base 64 de la chaine client_id:client_password>"
```


# Contrôle de l'authentification appel du Portail RUDI vers un fournisseur de données

Lorsque le portail vient moissonner des données d'un fournisseur de données ou lorsque le portail vient soumettre un rapport d'intégration, l'appel comporte une entête d'authorization de type "Bearer".

Exemple :
```
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJydWRpIiwiY29ubmVjdGVkVXNlciI6eyJsb2dpbiI6InJ1ZGkiLCJ0eXBlIjoiUEVSU09OIiwiZmlyc3RuYW1lIjoicnVkaSIsImxhc3RuYW1lIjoicnVkaSIsImVtYWlsIjpudWxsLCJvcmdhbml6YXRpb24iOiJydWRpIiwicm9sZXMiOlsiQURNSU5JU1RSQVRPUiJdfSwiZXhwIjoxNjE0NjE5Nzc2LCJpYXQiOjE2MTQ2MTYxNzZ9.Em7yclposciDOll-Dgv9O6jGDE-GsVEHp9dYKyfYNCyPTAambdGqtnl--Zw0DidCf0_JCghXlpznMIteUPdHnQ
```

Le fournisseur de données peut valider ce token en réalisant l'appel suivant vers le portail :

```
curl -v --request GET http://rudi.bzh/oauth/check_token?token=valeur du token>
```

# Authentification des porteurs de projets

Lorsqu'un porteur de projet souhaite utiliser un jeu de données exposé par le portail, il peut le faire
* soit en tant qu'utilisateur anonyme. Cette possibilité est proposée afin de permettre à un utilisateur de réaliser des essais rapidement
* soit en son nom. C'est le cas de tous les jeux de données à accès restreint et c'est aussi le cas si le porteur de projet souhaite une qualité de service particulière. Il est important de noter que pour effectuer ce mode d'authentification, il est nécessaire de soumettre un projet dans Rudi puis de souscrire aux différents jeux de données (pas encore disponible)


## Dans le cas d'une utilisation en tant qu'utilisateur anonyme, il faut :

* Les APIs de téléchargement sont requêtables comme suit :

Appel de l'API de téléchargement pour le media souscrit :
```
curl -v -X GET  "https://rudi.bzh/medias/uuid_du_média/dwnl/1.0.0" 
```

L'url de téléchargement d'un média d'un jeu de données est consultable depuis le détail du jeu de données, dans la partie Informations complémentaires / Sources de données. 

## Pour une accès en son nom propre (éléments en cours de définition coté portail et non disponible actuellement sur rudi.bzh), il faut :
* Se connecter sur le portail avec son compte utilisateur
* Accéder au détail du compte
* Activer l'utilisation des APIs
* Récupérer le couple "customer_key"/customer_secret"
* Utiliser ce couple pour s'authentifier :

```
curl -kv -X POST -H "Authorization: Basic [base64(customer_key:customer_secret)]" -d "grant_type=client_credentials&username=[login du user sur le portail associé au customer_key]&scope=apim:subscribe apim:app_manage" -H "Content-Type:application/x-www-form-urlencoded" "https://rudi.bzh/apm/token"
```

Cet appel permet de récupérer un token.

* à partir du token il est alors possible de requêter les APIs de téléchargement comme suit :

```
curl -kv -X GET -H "Authorization: Bearer [token]" "https://rudi.bzh/apm/medias/[uuid_du_média]/dwnl/1.0.0"
```
