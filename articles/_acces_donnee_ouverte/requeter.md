---
order: 2
title: Requêter un jeu de données ouvert
---

# Requêter un jeu de donnée ouvert

Il est possible de requêter un jeu de données ouvert sans être authentifié.
Il suffit de faire un GET sur l'url du jeu de données. Vous trouverez ci-dessous un exemple de requête :
```
curl -v -X GET "https://rudi.bzh/medias/fd6be827-a10f-46d0-9c58-e143d26f7e55/dwnl/1.0.0"
```

L'url d'un média est consultable depuis le portail, dans le détail d'un jeu de données / Informations complémentaires / Sources de données (un bouton vous permet de copier directement l'url).
