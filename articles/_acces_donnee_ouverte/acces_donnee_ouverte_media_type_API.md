---
order: 3
title: Requêter un média de type API
---

# Requêter un média de type API d'un jeu de données ouvert

Il est possible de requêter un jeu de données ouvert dont le média est de type "API".
Il suffit de faire un POST sur l'url d'un média du jeu de données ouvert de type API afin de récupérer le token de l'utilisateur Anonymous. Vous trouverez ci-dessous un exemple de requête :
```
curl -kv -X POST -H "Authorization: Basic NGZsNllNOXVVSFRScXNUTGNZSjN3cUVPNUI0YTpqN0ttV2hPYk1BakZwSFlESjdMQUx6cGQyMmNh" -d "grant_type=client_credentials&username=anonymous&scope=apim:subscribe apim:app_manage" -H "Content-Type:application/x-www-form-urlencoded"  "https://rudi.bzh/apm/token"
```
Vous récupérerez un token qu'il faudra intégrer dans une nouvelle requête GET. L'identifiant d'un média est aussi à ajouter, celui-ci est consultable depuis le portail, dans le détail d'un jeu de données / Informations complémentaires / Sources de données (un bouton vous permet de copier directement l'url) : 

![image](https://github.com/sigrennesmetropole/rudi_documentation/assets/109140019/320150b0-8ef3-455d-b37e-e63269fb88af)

Voici un exemple de requête vous permettant d'appeler un média de type API : 
```
curl -kv -XGET -H "Authorization: Bearer <le token wso2 que vous avez récupéré ci-dessus>" "https://rudi.bzh/apm/medias/INSERER ICI L'IDENTIFIANT DU MEDIA/dwnl/1.0.0"
```
