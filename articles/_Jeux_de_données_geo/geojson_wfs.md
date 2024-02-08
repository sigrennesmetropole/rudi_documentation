---
order: 4
title: Média WFS type GeoJson
---

# Publier un jeu de données ayant un média WFS type GeoJson

Vous trouverez ci-dessous la section available format à ajouter à votre json de publication. 

Point Infos : Pour récupérer un json d'un jeu de données présent sur rudi.bzh vous pouvez le requêter en effectuant un GET sur l'url d'un média présent dans la page de détail du jeu de données (section : "Source de données"). 
* La requête : curl -v -X GET "https://rudi.bzh/medias/fd6be827-a10f-46d0-9c58-e143d26f7e55/dwnl/1.0.0".

La section de code à ajouter au JSON : 

```
    "available_formats": [
        {
            "media_type": "FILE",
            "media_id": "{{$guid}}",
            "file_type": "application/geo+json",
            "file_size": 4096,
            "file_encoding": "UTF-8",
            "checksum": {
                "algo": "MD5",
                "hash": "bc4711a0-6c95-4f0f-b84d-534186ae8a89"
            },
            "connector": {
                "interface_contract": "dwnl",
                "url": "https://public.sig.rennesmetropole.fr/geoserver/ptou_lois/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ptou_lois%3Acircuit_pedestre&maxFeatures=200&outputFormat=application%2Fjson",
                "connector_parameters": [
                    {
                        "key": "layer",
                        "value": "ptou_lois:circuit_pedestre",
                        "type": "STRING",
                        "usage": "Nom du layer à récupérer",
                        "accepted_values": [
                            {
                                "cle": "layer"
                            }
                        ]
                    },
                    {
                        "key": "versions",
                        "value": "1.0.0",
                        "type": "STRING",
                        "accepted_values": [
                            {
                                "cle": "versions"
                            }
                        ]
                    },
                    {
                        "key": "default_crs",
                        "value": "EPSG:3948",
                        "type": "STRING",
                        "accepted_values": [
                            {
                                "cle": "default_crs"
                            }
                        ]
                    },
                    {
                        "key": "formats",
                        "value": "application/json",
                        "type": "STRING",
                        "accepted_values": [
                            {
                                "cle": "formats"
                            }
                        ]
                    }
                ]
            }
        }
    ],

```

