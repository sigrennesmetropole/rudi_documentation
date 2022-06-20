---
order: 3
title: API exposées
---

Le portail Rudi expose des microservices pour sa propre utilisation mais ils peuvent également être utilisés par tous en s'authentifiant ou non.

# Microservices exposés par le portail Rudi

Les microservices exposés sont listés ci-dessous. Leur documentation est accessible via les hyperliens.

* [acl](https://rudi.bzh/acl/swagger-ui/index.html?configUrl=/acl/v3/api-docs/swagger-config) : microservice permettant d'administrer les utilisateurs
* [konsult](https://rudi.bzh/konsult/swagger-ui/index.html?configUrl=%2Fkonsult%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=konsult) : microservice permettant d'administrer les jeux de données du catalogue Rudi
* [projekt](https://rudi.bzh/projekt/swagger-ui/index.html?configUrl=%2Fprojekt%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=projekt) : microservice permettant d'administrer les projets dans Rudi
* [kos](https://rudi.bzh/kos/swagger-ui/index.html?configUrl=%2Fkos%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=kos) : microservice permettant d'administrer les mots-clés et thèmes au format SKOS
* [kalim](https://rudi.bzh/kalim/swagger-ui/index.html?configUrl=%2Fkalim%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=kalim) : microservice permettant de publier/modifier/supprimer un jeu de données dans le portail
* [providers](https://rudi.bzh/providers/swagger-ui/index.html?configUrl=%2Fproviders%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=providers) : microservice permettant d'administrer les producteurs de données

# API de catalogage
Le micoservice konsult expose une API de catalogage des jeux de données Rudi : [API catalogage](https://rudi.bzh/konsult/swagger-ui/index.html?configUrl=%2Fkonsult%2Fv3%2Fapi-docs%2Fswagger-config&urls.primaryName=konsult#/datasets/searchMetadatas).

Pour l'utiliser, il est nécessaire de s'authentifier au près du portail en tant qu'anonymous ou avec votre compte utilisateur et de récupérer un token JWT Rudi :

<pre>
curl -v -X POST https://rudi.bzh/token -d "grant_type=password&username=anonymous&password=anonymous" -H "Authorization: Basic TEgxT1o1T3JMZmRFcXlRdkozcEFvUzhieFFNYTpYYWdmOENRdEpzak1UV09pdnBueGxjbTczb0lh"
</pre>

A partir du token il est alors possible de requêter l'API de catalogage comme suit :

<pre>
curl -v -X GET  "https://rudi.bzh/konsult/v1/datasets/metadatas" -H "Authorization: Bearer [l'access token retourné par l'appel précédent]" 
</pre>
