---
order: 5
title: Déchiffrement des données
---

# Déchiffrement des données en accès restreint

Les données en accès restreint sont stockées chiffrées sur le noeud producteur. Pour chaque média chiffré les connector_parameters suivants doivent être renseignés :

- `encrypted` qui doit toujours être à `true` ;
- `public_key_url` : l'URL de la clé publique utilisée ;
- `pub_key_cut` : les premièrs caractères du contenu de la clé publique utilisée pour chiffrer (si le paramètre précédent n'est pas renseigné) ;
- `original_mime_type` : le MIME type avant chiffrement du média ;
- `encrypted_mime_type` : le MIME type après chiffrement du média.

Exemple de média :

```json
	"connector_parameters": [
		{
			"key": "encrypted",
			"value": "true",
			"type": "BOOLEAN"
		},
		{
			"key": "pub_key_cut",
			"value": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvS3nTZOj01kq1V6wKpMe",
			"type": "STRING"
		},
		{
			"key": "original_mime_type",
			"value": "image/jpeg",
			"type": "STRING"
		},
		{
			"key": "encrypted_mime_type",
			"value": "image/jpeg+crypt",
			"type": "STRING"
		}
	]
```

## Chiffrement à partir de la clé publique du portail
Elles peuvent être chiffrées à partir d'une clé publique exposée par le portail.
Cette clé est accessible via la requête :
```
curl -X GET "https://rudi.bzh/konsult/v1/encryption-key" -H  "accept: application/octet-stream"
```

Dans ce cas là, le déchiffrement des données se fait automatiquement par le portail lors d'un téléchargement. Cette étape est totalement transparente pour vous.

## Chiffrement à partir d'une clé inconnue pour le portail
Les données en accès restreint peuvent être chiffrées à partir d'une autre clé, qui n'est pas connue par le portail.
Dans ce cas, le portail Rudi ne peut pas déchiffrer la donnée. Les données que vous récupérez au moment du téléchargement seront alors chiffrées.
