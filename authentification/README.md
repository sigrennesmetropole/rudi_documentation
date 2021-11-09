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

Lorsqu'un portail de projet souhaite utiliser un jeu de données exposé par le portail, il peut le faire 
* soit en son nom. C'est le cas de tous les jeux de données à accès restreint et c'est aussi le cas si le porteur de projet souhaite une qualité de service particulière. Il est important de noter que pour effectuer ce mode d'authentification, il faut un utilisateur Rudi (création de compte depuis le portail) et que cet utilisateur est souscrit aux différents jeu de données.
* soit en tant qu'utilisateur anonyme. Cette possibilité est proposée afin de permettre à un porteur de réaliser des essais rapidement. Le mot de passe de cet utilisateur est "anonymous"

La procédure est la même dans les 2 cas :
* Récupération d'un client_id/client_secret (cette opération n'est à faire qu'une seule fois)

<pre>
curl -X POST -H "Authorization: Basic [base64(login:mot de passe)]" -k -v -H "Content-Type: application/json" -d @payload.json https://rudi.bzh/wso2/client-registration/v0.17/register > clientkey.json
</pre>

Le contenu du payload est:
<pre>
 {
  "callbackUrl":"www.google.lk",
  "clientName":"rest_api_admin",
  "owner":"_mettre ici le login_",
  "grantType":"client_credentials password refresh_token",
  "saasApp":true
}
</pre>

* Récupération d'un token 

<pre>
curl -v -X POST -H "Authorization: Basic Basic [base64(client_id:client_secret)]" -k -d "grant_type=password&username=<login>&password=<password>&scope=apim:api_view" -H "Content-Type:application/x-www-form-urlencoded" https://wso2.open-dev.com:9443/oauth2/token > token-user1.json
</pre>


