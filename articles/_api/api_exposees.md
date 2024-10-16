---
order: 3
title: API exposées
---

Le portail Rudi expose des microservices pour sa propre utilisation mais ils peuvent également être utilisés par tous en s'authentifiant ou non.

# Microservices exposés par le portail Rudi

Les microservices exposés sont listés ci-dessous. Leur documentation est accessible via les hyperliens.

* [acl](https://rudi.rennesmetropole.fr/acl/swagger-ui/index.html?configUrl=/acl/v3/api-docs/swagger-config) : microservice permettant d'administrer les utilisateurs
* [konsult](https://rudi.rennesmetropole.fr/konsult/swagger-ui/index.html?configUrl=%2Fkonsult%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=konsult) : microservice permettant d'administrer les jeux de données du catalogue Rudi
* [projekt](https://rudi.rennesmetropole.fr/projekt/swagger-ui/index.html?configUrl=%2Fprojekt%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=projekt) : microservice permettant d'administrer les projets dans Rudi
* [kos](https://rudi.rennesmetropole.fr/kos/swagger-ui/index.html?configUrl=%2Fkos%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=kos) : microservice permettant d'administrer les mots-clés et thèmes au format SKOS
* [kalim](https://rudi.rennesmetropole.fr/kalim/swagger-ui/index.html?configUrl=%2Fkalim%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=kalim) : microservice permettant de publier/modifier/supprimer un jeu de données dans le portail
* [strukture](https://rudi.rennesmetropole.fr/strukture/swagger-ui/index.html?configUrl=%2Fstrukture%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=strukture) : microservice permettant d'administrer les producteurs de données

# API de catalogage des jeux de données
Le microservice konsult expose une API de catalogage des jeux de données Rudi : [API catalogage](https://rudi.rennesmetropole.fr/konsult/swagger-ui/index.html?configUrl=%2Fkonsult%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=konsult#/datasets/searchMetadatas).

Pour l'utiliser, il est nécessaire de s'authentifier au près du portail en tant qu'anonymous ou avec votre compte utilisateur et de récupérer un access token  Rudi :

```
curl -v -X POST "https://rudi.rennesmetropole.fr/authenticate" --header "Content-Type: application/x-www-form-urlencoded" --data-urlencode "login=anonymous" --data-urlencode "password=anonymous"
```

A partir du token il est alors possible de requêter l'API de catalogage comme suit :

```
curl -v -X GET  "https://rudi.rennesmetropole.fr/konsult/v1/datasets/metadatas" -H "Authorization: Bearer [l'access token retourné par l'appel précédent]"
```  

# API pour récupérer la clé publique de chiffrement
Le micoservice konsult expose une API permettant de récupérer la clé publique pour chiffrer les données : [API clé publique](https://rudi.rennesmetropole.fr/konsult/swagger-ui/index.html?configUrl=%2Fkonsult%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=konsult#/encryption-key/getEncryptionKey).

La clé publique est récupérable de la manière suivante :
```
curl -X GET "https://rudi.bzh/konsult/v1/encryption-key" -H  "accept: application/octet-stream"
```  
