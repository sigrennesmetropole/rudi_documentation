---
order: 5
title: Déchiffrement des données
---

Les données restreintes sont stockées chiffrées sur le noeud producteur.

## Chiffrement à partir de la clé publique du portail
Elles peuvent être chiffrées à partir d'une clé publique exposée par le portail.
Cette clé est accessible via la requête :
```
curl -X GET "https://rudi.bzh/konsult/v1/encryption-key" -H  "accept: application/octet-stream"
```

Dans ce cas là, le déchiffrement des données se fait automatiquement par le portail lors d'un téléchargement. Cette étape est totalement transparente pour vous.

## Chiffrement à partir d'une clé inconnue pour le portail
Les données restreintes peuvent être chiffrées à partir d'une autre clé, qui n'est pas connue par le portail.
Dans ce cas, le portail ne peut pas déchiffrer la donnée. Les données que vous récupérez au moment du téléchargement seront alors chiffrées.
