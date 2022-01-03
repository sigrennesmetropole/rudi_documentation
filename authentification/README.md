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
* soit en son nom. C'est le cas de tous les jeux de données à accès restreint et c'est aussi le cas si le porteur de projet souhaite une qualité de service particulière. Il est important de noter que pour effectuer ce mode d'authentification, il faut un utilisateur Rudi (création de compte depuis le portail) et que cet utilisateur ait souscrit aux différents jeux de données.
* soit en tant qu'utilisateur anonyme. Cette possibilité est proposée afin de permettre à un porteur de réaliser des essais rapidement. Le mot de passe de cet utilisateur est "anonymous" et son login est "anonymous".

La procédure est la même dans les 2 cas :
* Récupération d'un client_id/client_secret (cette opération n'est à faire qu'une seule fois)

<pre>
curl -X POST -H "Authorization: Basic [base64(login:mot de passe)]" -k -v -H "Content-Type: application/json" -d @payload.json https://rudi.bzh/client-registration/v0.17/register > clientkey.json
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

Le réponse contient les champs _client_id_ et _client_secret_ utilisable pour les opérations suivantes

* Récupération d'un token 

<pre>
curl -v -X POST "https://rudi.bzh/token" -d "grant_type=password&username=[client_id]&password=[client_secret]" -H "Authorization: Basic [base64(client_id:client_secret)]"
</pre>

Le token est de la forme :
<pre>
{"access_token":"eyJ4NXQiOiJNell4TW1Ga09HWXdNV0kwWldObU5EY3hOR1l3WW1NNFpUQTNNV0kyTkRBelpHUXpOR00wWkdSbE5qSmtPREZrWkRSaU9URmtNV0ZoTXpVMlpHVmxOZyIsImtpZCI6Ik16WXhNbUZrT0dZd01XSTBaV05tTkRjeE5HWXdZbU00WlRBM01XSTJOREF6WkdRek5HTTBaR1JsTmpKa09ERmtaRFJpT1RGa01XRmhNelUyWkdWbE5nX1JTMjU2IiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJhbm9ueW1vdXMiLCJhdXQiOiJBUFBMSUNBVElPTl9VU0VSIiwiYXVkIjoiSEQwZnVmak1YaDdUYUp1bDM0OEd2TF8zY25jYSIsIm5iZiI6MTYzNjU0MTA0OSwiYXpwIjoiSEQwZnVmak1YaDdUYUp1bDM0OEd2TF8zY25jYSIsImlzcyI6Imh0dHBzOlwvXC9sb2NhbGhvc3Q6OTQ0M1wvb2F1dGgyXC90b2tlbiIsImV4cCI6MTYzNjU0NDY0OSwiaWF0IjoxNjM2NTQxMDQ5LCJqdGkiOiI1YWEyYzRkYy1jMGZhLTQ2ODItYjJiNi1mODAwMmJmODExYjAifQ.p0lNY8SClAVqUCWdH6ImWCUTOp_qnqnGROP1fcpbKgxUK5KSBovqMbiRkKXtZeCs6BLOPgXxEBgy7FqtlROB1jkOs_n-sB6tjTzGvKrdCruJT4nJGVP5toUVqALsTP_rHWFsN6l_llFUymxxaLkGDSR9b_mxjyh5_8R39I7qhH4TM58icJdc9WcIaBVgxky8suJzmZ3QvZT49_toFbcRaawcPxDFwrXbLbxvdGAk1k2-cSj3Sm59a7pYZoCufQFcGPsag8UVswkKGT46qa3oVvf3G2cU9ceJLXefElwpw509pA80lMUwq4c58UpFQX28LRw-cK3DgDF7oX9EUnDn-w","refresh_token":"cd8e4abd-5a14-3bfc-9b93-c225a9535ec3","token_type":"Bearer","expires_in":3600}
</pre>

* Appel de l'API de téléchargement pour le media souscrit :

<pre>
curl -v -X GET  "https://rudi.bzh/medias/eef6832f-6a06-4f65-8f95-a533ac8926a7/dwnl/1.0.0" -H "Authorization: Bearer [l'access token retourné par l'appel précédent]" 
</pre>


