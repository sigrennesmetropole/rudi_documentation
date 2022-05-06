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

<pre>
curl -v --request POST http://&lt;server>:&lt;port>/oauth/token --data "grant_type=password" --data "username=&lt;username>" --data "password=&lt;client_password>" --data "scope=&lt;liste des scopes séparés par des virgules>" --data "client_id=&lt;client_id>" -H "Authorization:Basic &lt;encodage en base 64 de la chaine &lt:client_id:client_password>"
</pre>


# Contrôle de l'authentification appel du Portail RUDI vers un fournisseur de données

Lorsque le portail vient moissonner des données d'un fournisseur de données ou lorsque le portail vient soumettre un rapport d'intégration, l'appel comporte une entête d'authorization de type "Bearer".

Exemple :
<pre>
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJydWRpIiwiY29ubmVjdGVkVXNlciI6eyJsb2dpbiI6InJ1ZGkiLCJ0eXBlIjoiUEVSU09OIiwiZmlyc3RuYW1lIjoicnVkaSIsImxhc3RuYW1lIjoicnVkaSIsImVtYWlsIjpudWxsLCJvcmdhbml6YXRpb24iOiJydWRpIiwicm9sZXMiOlsiQURNSU5JU1RSQVRPUiJdfSwiZXhwIjoxNjE0NjE5Nzc2LCJpYXQiOjE2MTQ2MTYxNzZ9.Em7yclposciDOll-Dgv9O6jGDE-GsVEHp9dYKyfYNCyPTAambdGqtnl--Zw0DidCf0_JCghXlpznMIteUPdHnQ
</pre>

Le fournisseur de données peut valider ce token en réalisant l'appel suivant vers le portail :

<pre>
curl -v --request GET http://&lt;server>:&lt;port>/oauth/check_token?token=&ltvaleur du token>
</pre>

# Authentification des porteurs de projets

Lorsqu'un porteur de projet souhaite utiliser un jeu de données exposé par le portail, il peut le faire
* soit en tant qu'utilisateur anonyme. Cette possibilité est proposée afin de permettre à un utilisateur de réaliser des essais rapidement
* soit en son nom. C'est le cas de tous les jeux de données à accès restreint et c'est aussi le cas si le porteur de projet souhaite une qualité de service particulière. Il est important de noter que pour effectuer ce mode d'authentification, il est nécessaire de soumettre un projet dans Rudi puis de souscrire aux différents jeux de données (pas encore disponible)


## Dans le cas d'une utilisation en tant qu'utilisateur anonyme, il faut :

* Les APIs de téléchargement sont accessibles comme suit :

Appel de l'API de téléchargement pour le media souscrit :
<pre>
curl -v -X GET  "https://rudi.bzh/medias/uuid_du_média/dwnl/1.0.0" 
</pre>

L'uuid du média est accessible en regardant l'url envoyée au clic sur le bouton Télécharger dans le portail : uuid après /media.

Une évolution est en cours dans le portail pour rendre cette url plus facilement accessible.

## Pour une accès en son nom propre (éléments en cours de définition coté portail et non disponible actuellement sur rudi.bzh), il faut :
* Se connecter sur le portail avec son compte utilisateur
* Accéder au détail du compte
* Activer l'utilisation des APIs
* Récupérer le couple "customer_key"/customer_secret"
* Utiliser ce couple pour s'authentifier :

<pre>
curl -kv -X POST -H "Authorization: Basic [base64(customer_key:customer_secret)]" -d "grant_type=client_credentials&username=[login du user sur le portail associé au customer_key]&scope=apim:subscribe apim:app_manage" -H "Content-Type:application/x-www-form-urlencoded" https://rudi.bzh/apim/oauth2/token
</pre>

Cet appel permet de récupérer un token.

* à partir du token il est alors possible d'accéder aux APIs de téléchargement comme suit :

<pre>
curl -kv -X POST -H "Authorization: Bearer <token>" https://rudi.bzh/apim/datasets/02777fcb-c0bd-4d89-830a-8070cfb89261/dwnl/1.0.0
</pre>