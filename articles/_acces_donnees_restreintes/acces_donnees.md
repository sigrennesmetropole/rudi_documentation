---
order: 4
title: Accéder aux données
---
Une fois la demande d'accès acceptée par le producteur, vous pouvez requêter le jeu de données en effectuant les requêtes suivantes.

## Récupération d'un token

```
curl -kv -X POST -H "Authorization: Basic [base64(customer_key:customer_secret)]" -d "grant_type=client_credentials&username=[login du user sur le portail associé au customer_key]&scope=apim:subscribe apim:app_manage" -H "Content-Type:application/x-www-form-urlencoded" "https://rudi.bzh/apm/token"
```
Cet appel permet de récupérer un token.

## Téléchargement de la donnée
A partir du token il est alors possible de requêter les APIs de téléchargement comme suit :

```
curl -kv -X GET -H "Authorization: Bearer [token]" "https://rudi.bzh/apm/medias/[uuid_du_média]/dwnl/1.0.0"
```

L'uuid du média est récupérable depuis le détail du jeu de données, en consultant l'url mentionnée dans les sources de données.

Vous pouvez également télécharger le jeu de données depuis son détail : pour cela vous devez vous connecter, accéder au détail du jeu de donnée et cliquer sur le bouton Télécharger.
