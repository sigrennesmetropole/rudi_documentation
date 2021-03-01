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
