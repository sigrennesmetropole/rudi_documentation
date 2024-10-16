---
order: 2
title: Média WMTS
---

# Publier un jeu de données ayant un média de type WMTS

Vous trouverez ci-dessous la section available format à ajouter à votre json de publication. 

Point Infos : Pour récupérer un json d'un jeu de données présent sur rudi.bzh vous pouvez le requêter en effectuant un GET sur l'url d'un média présent dans la page de détail du jeu de données (section : "Source de données"). 
* La requête : curl -v -X GET "https://rudi.rennesmetropole.fr/medias/fd6be827-a10f-46d0-9c58-e143d26f7e55/dwnl/1.0.0".

La section de code à ajouter au JSON (particularité : Seule la projection 3857 permet une visualisation au sein du portail.): 

```

    "available_formats": [
        {
            "media_type": "SERVICE", //Correspond au type de media, ici il s'agit d'un service (ne pas changer) 
            "media_id": "{{$guid}}", //Correspond à l'uuid du media
            "connector": {
                "interface_contract": "wmts", //Correspond au type de service (ne pas changer)
                "url": "https://public.sig.rennesmetropole.fr/geowebcache/service/wmts", // Correspond au début d'une url récupérée sur un geoserver, ici nous nous arrêterons à "wmts"
                "connector_parameters": [
                    {
                        "key": "layer", //Correspond à la clé "layer" (ne pas changer)
                        "value": "ref_fonds:top25", // Correspond à la layer à afficher
                        "type": "STRING", // Correspond au format (ne pas changer)
                        "usage": "Nom du layer à récupérer", //Correspond à l'usage de la clé "layer" (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "layer" //Correspond à la clé "Layer" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "default_crs", //Correspond à la clé "Default_crs" (ne pas changer)
                        "value": "EPSG:3857", //Correspond à la projection (ne pas changer)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "default_crs" //Correspond à la clé "Default_crs" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "versions",//Correspond à la clé "Versions" (ne pas changer)
                        "value": "1.3.0", //Correspond à la version (cette information est à récupérer au sein de l'url affichée dans votre onglet geoserver)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "versions"  //Correspond à la clé "Versions" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "matrix_set", //Correspond à la clé "Matrix_set" (ne pas changer)
                        "value": "EPSG:3857", //Correspond à la projection (ne pas changer)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "usage": "Nom du matrix set conjoint à la projection",//Correspond à l'usage de la clé "matrix_set" (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "matrix_set"//Correspond à la clé utilisée (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "matrix_id_prefix", //Correspond à la clé "Matrix_id_prefixe" (ne pas changer)
                        "value": "EPSG:3857", //Correspond à la projection (ne pas changer)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "usage": "Nom du préfixe au matrix set conjoint au matrix set", //Correspond à l'usage de la clé "matrix_id_prefix" (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "matrix_id_prefix" //Correspond à la clé "matrix_id_prefix" (ne pas changer)
                            }
                        ]
                    },
                    {
                        "key": "formats", //Correspond à la clé "Formats" (ne pas changer)
                        "value": "image/png", //Correspond au format (ne pas changer)
                        "type": "STRING", //Correspond au format (ne pas changer)
                        "usage": "Format des images pour l'affichage", //Correspond à l'usage de la clé "format" (ne pas changer)
                        "accepted_values": [
                            {
                                "cle": "format" //Correspond à la clé "Format"
                            }
                        ]
                    }
                ]
            }
        }
    ],

```
