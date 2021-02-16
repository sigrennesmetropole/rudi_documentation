# *Modélisation de la sécurité dans RUDI*

#
# **1. Contexte**

L'objet du présent document est de définir les différents concepts du Modèle de Sécurité défini pour RUDI, et d'illustrer comment il s'articulent avec des exemples concrets.
Le document définit également la notion de qualité de service et comment elle est amenée à être configurée dans RUDI.

*(Ce document est en cours de rédaction, les différents points abordés sont encore au stade de proposition)*

# **1. Concepts**

## **1.1. Concepts préalables**

## **1.1. Concept “Entités”**

### **1.1.1. Définition**

Une Entité est une organisation dans RUDI, qui peut être soit un nœud Producteur de données, soit nœud Consommateur.

> *Exemple de nœud Consommateur: une organisation "Porteuse de projet" qui souhaite collecter les données d'un nœud Producteur de façon récurrente pour proposer un service par le biais d'une application) [TODO: exemple plus concret avec des noms d'entreprise]*

Pour une Entité Consommatrice qui souhaite accéder aux données d'une Entité Productrice avec des droits d'accès et/ou une qualité de service spécifique, va être définie une configuration d'accès particulière. Dans RUDI, la notion d'Entité sert donc à modéliser les deux acteurs (l'un Consommateur, l'autre Producteur) qui vont négocier un tel Contrat d'accès.

### **1.1.1. Configuration par défaut**

[TODO: à valider — les utilisateurs lambdas peuvent-ils demander un droit d'accès ou une QoS particuliers ?] 

[TODO: à valider] Les utilisateurs qui dans RUDI sont référencés mais n'appartiennent pas à une organisation particulière sont considérés comme appartenant à l'Entité "Default Entity". Chaque Producteur définit une configuration de sécurité par défaut pour de tels accès "ouverts".

### **1.1.1. Création d'une Entité**

Les Entités sont crées par la Portail RUDI, pour chaque nouveau nœud Producteur et tout nouveau Porteur de Projet.

## **1.1. Concept “Groupe”**

### **1.1.1. Définition**

Un Groupe rassemble les Utilisateurs d'une Entité Consommatrice qui ont les mêmes droits d'accès et la même qualité de service pour un Producteur donné.
En d'autres termes, une Entité Consommatrice qui souhaite bénéficier d'un ou plusieurs accès particuliers auprès d'une Entité Productrice de données va définir chez celui-ci autant de Groupes. 

Ex : 
- groupe "Webservices"
- groupe "Aministrateurs Consommateurs" 
- groupe "Backoffice Consommateur"

[TODO: à valider] Les utilisateurs qui dans RUDI sont référencés mais n'appartiennent pas à une organisation particulière sont considérés comme appartenant au groupe "Default Group". Une configuration de sécurité est définie par le Producteur pour de tels accès "ouverts".

## **1.1. Concept “Utilisateur”**
Un compte utilisateur est associé soit à une personne physique, soit un robot (ex: agent de requête)
Les utilisateurs (physiques ou virtuels) d'une même Entité Consommatrice sont rassemblés au sein de différents Groupes en fonction de leur accès aux données

## **1.1. Concept “Rôle”**
Un Rôle regroupe un ensemble de droits / permissions 

## **1.1. Accès aux données**

## **1.1. Contrat d'interface**

## **1.1. Qualité de service**

## **1.1. Contrat**

# **1. Workflows**
