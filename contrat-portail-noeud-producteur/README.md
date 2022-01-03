**Note Technique de spécification d&#39;interface**

#
# **1. Contexte**

L&#39;objet du présent document est de définir le contrat d&#39;interface en termes d&#39;API REST entre le portail RUDI et un nœud producteur RUDI.

Le portail RUDI étant décomposé en micro-services, la communication entre un nœud producteur et le portail RUDI se fait au travers du micro-services « µKollect ».

#
# **2. Collecte des métadonnées**

La collecte des métadonnées est réalisée par le module « Mise à jour catalogue ».

Le module fonctionne selon 2 modes :

- Mode « notification » (ou push depuis le nœud producteur)
- Mode « moissonnage » (ou pull depuis le portail)

## **1.1. Collecte en mode « Notification »**

En mode « **notification** », le diagramme de séquence d&#39;appel est le suivant :

![diagramme de séquence des notifications](metacatalogue-push.png)

*Figure 1 - Diagramme de séquence &quot;notification&quot;*

Comme l&#39;indique le schéma ci-dessus, le nœud producteur peut appeler le module de collecte (µKollect) du portail RUDI afin de déposer une demande de collecte. Cette demande comporte :

- L&#39;identifiant unique du jeu de données
- La nature de la modification : création, modification des métadonnées ou suppression d&#39;un jeu de données.
- Les métadonnées du jeu de données

Lors de la réception de ce message, le module de collecte va l&#39;ajouter dans la pile des demandes à traiter persistée en base de données et envoie un accusé de réception au nœud producteur.

Le module de collecte va dépiler de manière asynchrone la liste des messages afin de réaliser les opérations demandées.

A l&#39;issue du traitement, le module de collecte envoie au nœud producteur un rapport d&#39;intégration indiquant le résultat de l&#39;intégration et éventuellement des informations de qualité des métadonnées.

**1.2. Collecte en mode « Moissonnage »**

En mode « **moissonnage** », le diagramme de séquence d&#39;appel est le suivant :

![diagramme de séquence de moissonnage](metacatalogue-pull.png)

*Figure 2 - Diagramme de séquence &quot;moisonnage&quot;*

Comme l&#39;indique le schéma ci-dessus, le module de collecte des données du portail RUDI commence par appeler le nœud producteur afin de collecter par leur identifiant unique tous les jeux de données dont les métadonnées ont été modifiées depuis la dernière date de collecte réussie.

Les modifications peuvent être des créations, des mises à jour ou des suppressions de jeux de données.

Pour chaque jeu de données créé ou modifié qui a été identifié par l&#39;opération précédente, le module de collecte appelle le nœud producteur afin de récupérer les métadonnées.

Le module de collecte traite ensuite de manière asynchrone l&#39;intégration des données et la suppression des jeux de données.

Pour chaque jeu de données traité, le module de collecte envoi au nœud producteur d&#39;origine un rapport d&#39;intégration indiquant le résultat de l&#39;intégration.

Ce rapport d&#39;intégration à vocation à permettre à un nœud producteur de ne fonctionner que par moissonnage et d&#39;être ainsi informé qu&#39;un jeu de données particulier a été intégré dans le portail.

#
# **3. contrat d&#39;interface Portail / nœud PRODUCTEUR**

## **3.1. Périmètre**

Le contrat d&#39;interface RUDI/Producteur porte sur les grands thèmes suivants :

- Jeux de données
  - L&#39;interface proposée doit permettre au portail RUDI d&#39;accéder aux données des jeux de données publiés dans le portail RUDI par un nœud producteur.
  - Chaque jeu de données doit être identifié de manière unique au sein de RUDI.
- Métadonnées
  - L&#39;interface proposée doit permettre au portail RUDI :
  - La collecte des métadonnées d&#39;un jeu de données proposé par le producteur à partir de l&#39;identifiant unique
  - La notification de l&#39;intégration des métadonnées d&#39;un jeu de données
  - L&#39;interface proposée par RUDI doit permettre aux producteurs de notifier la modification des métadonnées d&#39;un jeu de données.

- Données
  - L&#39;interface proposée doit permettre au portail RUDI de récupérer les données d&#39;un jeu de données.
  - Cette récupération doit pouvoir être réalisée en fonction de l&#39;utilisateur humain ou non ayant demandé les données mais aussi si possible en prenant en compte les notions de pagination, de tri et de filtrage par critère temporel, géographique et par mots-clefs.

- Consentement
  - L&#39;interface proposée doit permettre au portail RUDI de transférer au producteur de données, les informations de consentement recueillies par RUDI auprès des utilisateurs. Ce consentement doit être pris en compte lors de la récupération des données.

- Supervision
  - L&#39;interface proposée doit permettre au portail RUDI de superviser le nœud du producteur afin de prendre en compte la qualité de service associée.

Pour certains producteurs, le contrat d&#39;interface doit aussi couvrir le thème suivant :

- Appariement

L&#39;interface proposée doit permettre au portail RUDI et au producteur de réaliser l&#39;appariement (mise en correspondance) d&#39;un utilisateur RUDI et d&#39;un utilisateur connu du système du producteur)

## **3.2. Schéma de données**

### **3.2.1. Format des dates**

Les dates sont au format **ISO-8601** et prennent la forme :

- YYYY-MM-DD pour les dates
  - Exemple : 2020-12-14

- YYYY-MM-DDTH24:MI:SS.nano pour les « date+heure » - ce format est nommé « Timestamp » dans le reste du document.
  - Exemple : 2020-12-14T09:56:34.592384024

- YYYY-MM-DDTH24:MI:SS.nano&lt;+/- timezone offset ou Z pour UTC&gt; pour les « date+heure avec time-zone » - ce format est nommé « Timestamp avec time-zone » dans le reste du document.
  - Exemple : 2020-12-14T09:56:34.592384024+0100

### **3.2.2. Rapport d&#39;intégration – #/components/schemas/Report**

#### **3.2.2.1. Généralité**

Le rapport d&#39;intégration est de la forme suivante :

    {
      'report\_id': '',
      'submission\_date': '',
      'treatment\_date': '',
      'method' : ('POST'|'PUT'|'DELETE')
      'version':'',
      'global\_id': '',
      'resource\_title': '',
      'integration\_status': ('OK'|'KO'),
      'comment': '',
      'errors': [
        {
          'error\_code': '',
          'error\_message': ''
          'field\_name': '',
        }
      ]
    }

#### **3.2.2.2 Détail des données**

Le tableau ci-dessous présente les données présentes dans le rapport d&#39;intégration et leurs caractéristiques.

| **Nom balise** | **Description** | **Niveau** | **Obligatoire** | **Type** | **Taille** |
| --- | --- | --- | --- | --- | --- |
| report\_id | Identifiant du rapport d&#39;intégration | 1 | Oui | UUID v4 | |
| submission\_date | Date de soumission de la demande d&#39;intégration | 1 | Oui | Timestamp | |
| treatment\_date | Date de traitement de l&#39;intégration du jeu de donnéesdans Rudi | 2 | Oui | Timestamp | |
| version | Numéro de la version de l&#39;API utilisée par le nœud producteur pour communiquer avec le portail RUDI | 1 | Oui | Numérique | 2 |
| method | Méthode de soumission utilisée | 1 | Oui | Enuméré | 6 |
| resource\_id | Identifiant du jeu de donnéesdans le système Rudi | 2 | Oui | UUID v4 | |
| title | Nom du jeu de données | 2 | Non | Texte | Identique à lAPI |
| Integration\_status | État de l&#39;intégration du jeu de donnéesdans Rudi. 2 valeurs possibles :·OK : l&#39;intégration du jeu de données s&#39;est bien déroulée·KO : l&#39;intégration du jeu de données est en erreur | 2 | Oui | Enuméré | 2 |
| comment | Commentaire sur l&#39;état de l&#39;intégration. Formaté.·Si etat\_integration = OK, commentaire = l&#39;intégration du jeu de données&quot;_nom\_jeu\_de\_donnee_&quot; s&#39;est bien déroulée le &quot;_date\_traitement\_jdd\_Rudi_&quot;.·Si etat\_integration = KO, commentaire = l&#39;intégration du jeu de données&quot;_nom\_jeu\_de\_donnee_&quot; ne s&#39;est pas déroulée correctement, le &quot;_date\_traitement\_jdd\_Rudi. Veuillez consulter les erreurs ci-dessous et après correction des erreurs, renvoyer votre jeu de données. Pour plus d&#39;information, vous pouvez contacter votre administrateur Rudi._&quot;. | 2 | Oui | Texte | 255 |
| errors | Liste des erreurs rencontrées lors de l&#39;intégration du jeu de données. L&#39;objectif est de lister l&#39;ensemble des erreurs rencontrées sur le jeu de données afin d&#39;éviter de multiple envoi pour un même jeu de données(à chaque envoi, une nouvelle erreur est rencontrée). | 2 | Fonction de « status » :·Non si etat\_integration = 1·Oui si etat\_integration = 0 | Liste dobjets | Infini |
| error\_code | Code technique de l&#39;erreur(Cf. [3.2.2.3](#_89z81nu8ziwl)) | 3 | Oui | Texte | 7 |
| field\_name | Nom du champ concerné par l&#39;erreur (le cas échéant) | 3 | Non | Texte | 30 |
| error\_message | Descriptif de l&#39;erreur(Cf [3.2.2.3](#_89z81nu8ziwl)) | 3 | Oui | Texte | 255 |

#### **3.2.2.3. Détail des erreurs possibles**

Le tableau ci-dessous liste les différents cas d&#39;erreur possibles.

| **code\_erreur** | **commentaire\_erreur** |
| --- | --- |
| ERR-101 | Le format du fichier de métadonnées transmis est incorrect. |
| ERR-102 | La balise &quot;_nom\_balise_&quot; est inconnue |
| ERR-103 | Le paramètre &quot;_nom\_parametre_&quot; est manquant |
| ERR-104 | Autres erreurs d&#39;intégration |
| ERR-105 | Erreur inconnue: l&#39;erreur n&#39;a pas été reconnue, veuillez contacter l&#39;administrateur Rudi afin d&#39;analyser l&#39;erreur. |
| ERR-106 | La version de metadonnées &quot;_api\_version_&quot; n'est pas supportée. La version courante est X.X.X |
| ERR-1XX | …. Erreurs de base, à définir. |
| ERR-201 | Le type du champ &quot;_nom\_champ_&quot; n&#39;est pas le bon (format attendu : &quot;_format attendu&quot;_ / format reçu : _&quot;format\_reçu&quot;_ ) |
| ERR-202 | Le champ &quot;_nom\_champ&quot;_ est manquant alors qu&#39;il est obligatoire. |
| ERR-203 | La longueur du champ dépasse la limite autorisée: la taille attendue est _&quot;taille\_champ\_Rudi&quot;_ pour &quot;_nom\_champ_&quot; alors que la longueur de la valeur envoyée est _&quot; taille\_champ\_envoye&quot;_). |
| ERR-2XX | …. Erreurs sur le type du champ, à définir. |
| ERR-301 | Des caractères ne sont pas acceptés dans le &quot;_nom\_champ_&quot; |
| ERR-302 | La valeur saisie ne correspond pas au référentiel des valeurs attendues pour le champ &quot;_nom\_champ_&quot; (valeur saisie : &quot;_valeur\_saisie_&quot; / référentiel attendu : &quot;_liste des valeurs attendue séparées par des virgules_&quot; |
| ERR-303 | La valeur saisie &quot;_valeur\_saisie_&quot; pour le champ &quot;_nom\_champ_&quot; ne correspond pas à un code de concept SKOS connu |
| ERR-304 | La valeur saisie &quot;_valeur\_saisie_&quot; pour le champ &quot;_nom\_champ_&quot; est déjà utilisée |
| ERR-3XX | … Erreur en lien avec les valeurs d&#39;un champ, à définir. |
| ERR-403 | Le nœud fournisseur authentifié n'est pas le créateur du jeu de données |
| ERR-500 | Une erreur technique est survenue. Veuillez contacter l'administrateur Rudi pour analyser l'erreur. |
| ERR-XXX | … Autres types d&#39;erreurs, autres erreurs, à définir. |

### **3.2.3. Identifiant Rudi**

L&#39;identifiant d&#39;un jeu de données RUDI est une chaîne de caractères composée comme suit :

    <UUID v4>

L&#39;expression régulière permettant de définir un tel champ est la suivante :

    /^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i

### **3.2.4**** Métadonnées d&#39;un jeu de données – Rudi #/components/schemas/Metadata**

Cet objet représente l&#39;ensemble des métadonnées d&#39;un jeu de données (Cf. [4.1](#_l2u37jhi2oiz)).

### **3.2.5. Liste de métadonnées – #/component/schemas/MetadataList**

    {
      'total': <int64>,
      'items': [
        #/components/schemas/Metadata
      ]
    }

| **Nom balise** | **Description** | **Niveau** | **Obligatoire** | **Type** | **Taille** |
| --- | --- | --- | --- | --- | --- |
| total | Nombre total d&#39;éléments répondant à la requête | 1 | Oui | Int64 | |
| items | Liste des métadonnées | 1 | Oui | List d&#39;objet de type Metadata | |

## **3.3. URL d&#39;accès et gestion du versionnement**

Les différentes API sont exposées avec une URL de la forme : &lt;host&gt;/&lt;prefixe&gt;

- &lt;prefixe&gt; prendra la forme api par défaut mais peut prendre d&#39;autres formes si plusieurs typologie d&#39;API sont exposées.
  - Exemple &lt;host&gt;/api

L&#39;API dans sa version la plus récente sera exposée en &lt;host&gt;/&lt;prefixe&gt;/*

Cela signifie que la version la plus récente de l&#39;API est accessible directement sans changement de code des appelants.

Les API dans les versions plus anciennes seront exposées en

    <host><prefixe><version>/*

- &lt;version&gt; prendra la forme v&lt;Majeur&gt; ou v&lt;Majeur&gt;.&lt;Mineur&gt;
  - Exemple : &lt;host&gt;/api/v1 ou &lt;host&gt;/api/v1.1

Cela signifie que les modifications de niveau « Révision » n&#39;ont pas d&#39;impact sur la version de l&#39;API.

Dans la mesure du possible, seul le niveau &lt;Majeur&gt; sera utilisé.

## **3.4. Contrat d&#39;interface Jeu de données &amp; Métadonnées**

### **3.4.1. Service Portail**

#### **3.4.1.1. URL &amp; préfixe**

Les différents services décrits ci-après seront accessibles avec le préfixe « api ».

- Exemple &lt;host&gt;/api/*

##

#### **3.4.1.2. Création d&#39;un jeu de données**

**Description** :

Soumission d&#39;une demande de création d&#39;un jeu de données par ses métadonnées

**POST &lt;&gt;/resources**

**Body** :

- #/components/schemas/Metadata

**Code retour** :

- 200 : prise en compte de la demande
  - report\_id : UUID V4
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

#### **3.4.1.3. Modification des métadonnées d&#39;un jeu de données**

**Description** :

Soumission d&#39;une demande de modification d&#39;un jeu de données par ses métadonnées

**PUT &lt;&gt;/resources**

**Body** :

- #/components/schemas/Metadata

**Code retour** :

- 200 : prise en compte de la demande
  - report\_id : UUID V4
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu dans la métadonnée est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

#### **3.4.1.4. Suppression d&#39;un jeu de données**

**Description :**

Soumission d&#39;une demande de suppression d&#39;un jeu de données

**DELETE &lt;&gt;/resources/{global\_id}**

**Code retour :**

- 200 : prise en compte de la demande
  - report\_id : UUID V4
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu dans la métadonnée est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

#### **3.4.1.5. Obtention d&#39;un Identifiant RUDI**

**Description :**

Demande de génération d&#39;un identifiant RUDI (UUID v4).

L'identifiant RUDI généré tiendra compte des informations contenues dans le token JWT d&#39;authentification pour déduire le producteur.

**GET &lt;&gt;/resources/id\_generation**

**Code retour :**

- 200 : prise en compte de la demande
  - #/components/schémas/RudiId
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu dans la métadonnée est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

### **3.4.2. Service Noeud**

#### **3.4.2.1. URL &amp; préfixe**

Les différents services décrits ci-après seront accessibles avec le préfixe « api ».

- Exemple &lt;host&gt;/api/*

#### **3.4.2.2. Recherche des jeux de données**

**Description :**

Recherche des jeux de données

**GET &lt;&gt;/resources**

**Query :**

- limit : int32
- offset : int32
- update\_date\_min : timestamp
- update\_date\_max : timestamp
- &lt;à compléter&gt;

**Code retour :**

- 200 : prise en compte de la demande
  - #/components/schémas/ResourceListInfo
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 410 :
  - $ref: &quot;#/components/responses/Error410Gone&quot;
- 423 :
  - $ref: &quot;#/components/responses/Error423Locked&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

&lt;à compléter&gt;

#### **3.4.2.3. Obtention d&#39;un jeu de données**

**Description :**

Obtention des métadonnées d&#39;un jeux de données

**GET &lt;&gt;/resources/{global\_id}**

**Code retour :**

- 200 : prise en compte de la demande
  - #/components/schémas/RudiId
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 410 :
  - $ref: &quot;#/components/responses/Error410Gone&quot;
- 423 :
  - $ref: &quot;#/components/responses/Error423Locked&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

#### **3.4.2.4. Réception d&#39;un résultat d&#39;intégration**

**Description :**

Réception de l&#39;intégration d&#39;une demande réalisée par les services [3.4.1.2](#_fy6l6rbd94q2), [3.4.1.3](#_gj0yzmmqjzoi), [3.4.1.4.](#_gd2wuvaz8kv1).

**PUT &lt;&gt;/resources/{global\_id}/report**

**Body :** #/components/schemas/Report

**Code retour :**

- 200 : Prise en compte du rapport
- 400 :
  - $ref: &quot;#/components/responses/Error400BadRequest&quot;
- 401 :
  - $ref: &quot;#/components/responses/Error401Unauthorized&quot;
- 403 :
  - $ref: &quot;#/components/responses/Error403Forbidden&quot;
- 404 : le global\_id contenu dans la métadonnée est inconnu
  - $ref: &quot;#/components/responses/Error404NotFound&quot;
- 406 :
  - $ref: &quot;#/components/responses/Error406NotAcceptable&quot;
- 408 :
  - $ref: &quot;#/components/responses/Error408RequestTimeout&quot;
- 429 :
  - $ref: &quot;#/components/responses/Error429TooManyRequests&quot;
- 500 :
  - $ref: &quot;#/components/responses/Error500InternalServer&quot;
- 503 :
  - $ref: &quot;#/components/responses/Error503ServiceUnavailable&quot;

Les codes suivants impliquent que le portail doit retenter de notifier le nœud producteur du traitement de sa demande :

400, 401, 408, 429, 503

Le portail retentera l&#39;opération 5 fois à 1 heure d&#39;intervalle.

Au-delà de ce délai, une alerte doit être remontée côté portail pour une prise charge humaine.

### **3.4.3. Exemple de séquencement des appels**

#### **3.4.3.1. Notification**

Le mécanisme de notification prend place lorsqu&#39;un nœud producteur souhaite informer le portail de la publication (ou republication) d&#39;un jeu de données.

Par exemple, lors de la création d&#39;un nouveau jeu de données par le producteur, le séquencement des appels entre portail et nœud producteur est le suivant :

- Obtention par le nœud d&#39;un token JWT. Ce Point sera détaillé par ailleurs
- Si le producteur le souhaite, il peut demander un identifiant unique auprès du portail (GET &lt;&gt;/resources/generation\_id). Cet identifiant unique est appelé « global\_id ».
- Attribution par le nœud producteur de l&#39;identifiant au nouveau jeu de données
- Soumission par le nœud d&#39;une demande de création (POST &lt;&gt;/resources) avec la structure de métadonnées attendue
  - Réponse du portail par un code 200 avec un identifiant unique correspondant à la demande. Cet identifiant est appelé « report\_id ».
- Le portail traite la demande de manière asynchrone (mais dans l&#39;ordre des demandes) et à l&#39;issue de ce traitement le portail appelle le nœud d&#39;origine pour lui transmettre le rapport d&#39;intégration (PUT &lt;&gt;/resources/report/{global\_id}
  - Réponse du portail par un code 200
  - Ou pour les codes 400, 401, 408, 429, 503, 5 nouvelles tentatives possibles espacées d&#39;une heure

#### **3.4.3.2. Moissonnage**

Le mécanisme de moissonnage intervient lorsqu&#39;un nœud producteur ne met pas en œuvre le mécanisme de notification. Dans ce cas de figure, le portail, pour réaliser la mise à jour des métadonnées des différentes jeux, vient chercher, sur chaque nœud producteur, les jeux de données dont les métadonnées ont changé.

Le séquencement des appels entre Portail et nœud producteur est le suivant :

- Obtention par le portail d&#39;un token JWT
- Demande auprès du nœud par le portail des différents jeux de données modifiés depuis une date connue du portail (GET &lt;&gt;/resources avec des paramètres et de la pagination)
  - Réponse du nœud par un code 200 avec une liste d&#39;éléments
- Pour chaque élément reçu grâce à l&#39;appel précédent, contrôle de la date de dernière modification par rapport à la date connue du portail pour ce jeu de données
- Si le jeu de données doit être mis à jour, récupération des métadonnées du jeu (GET &lt;&gt;/resources/{global\_id})
  - Réponse du nœud par un code 200 avec les métadonnées du jeu de données
- Enregistrement dans la file d&#39;attente d&#39;une activité de mise à jour (création/modification/suppression)
- Le portail traite la demande et à l&#39;issue de ce traitement le portail appelle le nœud d&#39;origine pour lui transmettre le rapport d&#39;intégration (PUT &lt;&gt;/resources/report/{global\_id}
  - Réponse du portail par un code 200 ou
  - Ou pour les codes 400, 401, 408, 429, 503, 5 nouvelles tentatives possibles espacées d&#39;une heure

#
# **4. Annexes**

## **4.1. Annexe – Structure des métadonnées**

Version de l&#39;API au moment de l&#39;écriture de ce document:

[https://app.swaggerhub.com/apis/OlivierMartineau/RUDI-PRODUCER/1.0.3](https://app.swaggerhub.com/apis/OlivierMartineau/RUDI-PRODUCER/1.0.3)

Dernière version de l&#39;API:

[https://app.swaggerhub.com/apis/OlivierMartineau/RUDI-PRODUCER](https://app.swaggerhub.com/apis/OlivierMartineau/RUDI-PRODUCER/)
