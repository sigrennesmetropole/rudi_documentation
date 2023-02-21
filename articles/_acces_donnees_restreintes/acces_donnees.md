---
order: 4
title: Accéder aux données en accès restreint
---

# Accéder aux données en accès restreint

Une fois la demande d'accès acceptée par le producteur et la souscription réalisée, vous pouvez accéder au jeu de données de 2 manières :
* soit via API,
* soit directement dans le portail.

## Téléchargement de la donnée via API
**S'authentifier afin de récupérer un token**

La première étape pour télécharger un jeu de données via API est de s'authentifier en exécutant la commande suivante :
```
curl -kv -X POST -H "Authorization: Basic [base64(consumer_key:consumer_secret)]" -d "grant_type=client_credentials&username=[login du user sur le portail associé au customer_key]&scope=apim:subscribe apim:app_manage" -H "Content-Type:application/x-www-form-urlencoded" "https://rudi.bzh/apm/token"
```
Cet appel permet de récupérer un token.

Si vous souhaitez télécharger un jeu de données ouvert par le biais d'anonymous, vous pouvez récupérer le token en exécutant la commande suivante : 
```
curl -v -X POST "https://rudi.bzh/authenticate" --header "Content-Type: application/x-www-form-urlencoded" --data-urlencode "login=anonymous" --data-urlencode "password=anonymous"
```

**Télécharger le jeu de données**

A partir du "jwtToken "il est alors possible de requêter les APIs de téléchargement par le biais d'une requête "Get". Nous l'effectuons sur une url dont les 3 dernières informations sont :  "../Identifiant du fichier / Le type de fichier/ la version"

Exemple de requête sur le média de type csv du jeu de données : https://rudi.bzh/catalogue/detail/6c3b795c-0b60-4bf8-911c-c6f0625b7123 : 

```
curl -v -X GET " https://rudi.bzh/konsult/v1/medias/1cd92470-77b6-46c6-ae7b-14d10fac49d7/csv/1.0.0" -H "Authorization: Bearer [jwtToken]"
```

Les différentes informations du médias sont récupérables dans la page de détail du jeu de données au sein de la partie 'Informations complémentaires' puis 'Source de données'.

![image](https://user-images.githubusercontent.com/109140019/202900288-f8872540-c382-4308-8537-afb5503f7ed2.png)

_affichage des sources sur la page de détails d'un jeu de données_


## Téléchargement de la donnée à partir du portail
Vous pouvez également télécharger le jeu de données depuis son détail : pour cela vous devez vous connecter, accéder au détail du jeu de donnée et cliquer sur le bouton Télécharger.
