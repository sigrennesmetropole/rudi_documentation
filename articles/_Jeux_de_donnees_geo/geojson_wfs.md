---
order: 4
title: Média WFS type GeoJson
---

# Publier un jeu de données ayant un média WFS type GeoJson

Vous trouverez ci-dessous la section available format à ajouter à votre json de publication. 

Point Infos : Pour récupérer un json d'un jeu de données présent sur rudi.bzh vous pouvez le requêter en effectuant un GET sur l'url d'un média présent dans la page de détail du jeu de données (section : "Source de données"). 
* La requête : curl -v -X GET "https://rudi.rennesmetropole.fr/medias/fd6be827-a10f-46d0-9c58-e143d26f7e55/dwnl/1.0.0".

La section de code à ajouter au JSON : 

```
    "available_formats": [
        {
            "media_type": "FILE", //Correspond au type de media, ici il s'agit d'un fichier (ne pas changer)
            "media_id": "{{$guid}}", //Correspond à l'uuid du media
            "file_type": "application/geo+json", //Correspond au type de média (ne pas changer)
            "file_size": 4096, //Correspond à la taille du média
            "file_encoding": "UTF-8", //Correspond à l'encodage du média
            "checksum": {
                "algo": "MD5",
                "hash": "bc4711a0-6c95-4f0f-b84d-534186ae8a89" //Correspond à l'algorithme de hachage
            },
            "connector": {
                "interface_contract": "dwnl", //Correspond à l'interface contract (ne pas changer)
                "url": "https://public.sig.rennesmetropole.fr/geoserver/ptou_lois/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ptou_lois%3Acircuit_pedestre&maxFeatures=200&outputFormat=application%2Fjson", //Correspond à l'url du média (elle doit être structurée de la même façon)
                "connector_parameters": [
                    {
                        "key": "layer", //Correspond à la clé "layer" (ne pas changer)
                        "value": "ptou_lois:circuit_pedestre",  //Correspond à la couche que vous souhaitez importer
                        "type": "STRING", //Correspond au type (ne pas changer)
                        "usage": "Nom du layer à récupérer", //Correspond à l'usage de la clé layer
                        "accepted_values": [
                            {
                                "cle": "layer" //Correspond à la clé "Layer"
                            }
                        ]
                    },
                    {
                        "key": "versions", //Correspond à la clé "Versions"
                        "value": "1.0.0", //Correspond à la version présente dans l'url que vous avez renseigné
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "versions" //Correspond à la clé "Versions" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "default_crs", //Correspond à la clé "default_crs" (ne pas changer)
                        "value": "EPSG:3948", //Correspond à la projection 
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "default_crs" //Correspond à la clé "default_crs" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "formats", //Correspond à la clé "formats" (ne pas changer)
                        "value": "application/json", //Correspond au format (ne pas changer)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "formats" //Correspond à la clé "formats" (ne pas changer)
                            }
                        ]
                    }
                ]
            }
        }
    ],

```

