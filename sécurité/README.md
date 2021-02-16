### *Contrôle d'accès dans RUDI*

L'objet du présent document est de définir les différents concepts du modèle de contrôle d'accès défini pour RUDI, et d'illustrer comment il s'articulent avec des exemples concrets.
Le document définit également la notion de qualité de service et comment elle est amenée à être configurée dans RUDI.

*(Ce document est en cours de rédaction, les différents points abordés sont encore au stade de proposition)*

# **Concepts préalables**

> [TODO: peut-être mettre ça dans un autre document et y faire référence ?]

**Producteur** *(Producer)* : organisation identifiée dans le système RUDI qui produit des données.

**Fournisseur** *(Publisher)* : organisation identifiée dans le système RUDI qui produit des métadonnées et publie des données. Il s'agit généralement, mais pas nécessairement, du Producteur lui-même.

**Hébergeur** *(Host)* : organisation identifiée dans le système RUDI qui gère l'hébergement des données.

**Noeud Producteur** *(Producer node)* : du point de vue du système RUDI, on appèlera "Nœud Producteur" l'ensemble Producteur + Fournisseur + Hébergeur. Concrètement, le Fournisseur aura la charge de publier les données, mais aussi de gérer le contrôle d'accès aux données. C'est cette organisation qui gère les différentes actions attribuées ci-dessous à l'Entité Productrice car elle est l'interface du Nœud Producteur du point de vue du ystème RUDI.

**Consommateur** *(Consumer)* : le consommateur de données est l'utilisateur des données mises en partage dans RUDI, qui va les exploiter soit pour son propre usage, soit pour offrir des services à des tiers.

**Porteur de projet** *(Project holder)* : dans RUDI, un Porteur de projet est une organisation qui exploite les données pour offrir un service à des tiers, en général sous forme d'application.

# **Modélisation du contrôle d'accès dans RUDI**

## **Concept “Entité”**

### **Définition**

Une Entité est une organisation dans RUDI, qui peut être soit un nœud Producteur de données, soit nœud Consommateur.

> *Exemple de nœud Consommateur: une organisation "Porteuse de projet" qui souhaite collecter les données d'un nœud Producteur de façon récurrente pour proposer un service par le biais d'une application) [TODO: exemple plus concret avec des noms d'entreprise]*

Pour une Entité Consommatrice qui souhaite accéder aux données d'une Entité Productrice avec des droits d'accès et/ou une qualité de service spécifique, va être définie une configuration d'accès particulière. Dans RUDI, la notion d'Entité sert donc à modéliser les deux acteurs (l'un Consommateur, l'autre Producteur) qui vont négocier un tel Contrat d'accès.

### **Workflow: création d'une Entité**

Une Entité est créée au niveau du Portail RUDI, pour chaque nouveau nœud Producteur et tout nouveau Porteur de Projet.

> [TODO: qui crée ? API sur le Portail ? Procédure de vérification ?]

Une Entité ne peut être supprimée mais peut être désactivée 

> [TODO: par qui? Protail et/ou admin de l'Entité? Critères? conséquences pour les données/métadonnées?]

> [TODO: modification d'une Entité]

## **Concept “Groupe”**

### **Définition**

Un Groupe rassemble les Utilisateurs d'une Entité Consommatrice qui ont les mêmes droits d'accès et la même qualité de service pour un Producteur donné.
En d'autres termes, une Entité Consommatrice qui souhaite bénéficier d'un ou plusieurs accès particuliers auprès d'une Entité Productrice de données va définir chez celui-ci autant de Groupes. 

Ex : 
- groupe "Webservices"
- groupe "Administrateurs Consommateurs" 
- groupe "Backoffice Consommateur"

### **Workflow: création d'un Groupe**


## **Concept “Utilisateur”**
Un compte utilisateur est associé soit à une personne physique, soit un robot (ex: agent de requête)
Les utilisateurs (physiques ou virtuels) d'une même Entité Consommatrice sont rassemblés au sein de différents Groupes en fonction de leur accès aux données


## **Utilisateurs "citoyens" et Open Data**
> [TODO: à valider]
Les utilisateurs identifiés dans RUDI mais n'appartenant pas à une organisation particulière sont considérés comme appartenant à l'Entité "Default Entity" et au Groupe "Default Group".
Chaque Producteur doit définir une configuration de sécurité par défaut pour de tels accès "ouverts" (cette configuration peut éventuellement restreindre complètement l'accès aux données mais doit être définie).

> [TODO: à valider — les utilisateurs lambdas peuvent-ils demander un droit d'accès ou une QoS particuliers ? Si oui, création dynamique d'un nouveau Groupe dans "Default Entity" ?] 
 
## **Concept “Rôle”**
Un Rôle regroupe un ensemble de droits / permissions 

## **Accès aux données**
L'accès à un sous-ensemble des données d'une Entité Productrice est défini pour chaque Groupe d'une Entité Consommatrice donnée.

## **Contrat d'interface**

## **Qualité de service**

## **Contrat**
