**Modèle de sécurité**

#
# **1. Contexte**

L'objet du présent document est de définir les différents concepts du Modèle de Sécurité défini pour RUDI, et d'illustrer comment il s'articulent avec des exemples concrets.
Le document définit également la notion de qualité de service et comment elle est amenée à être configurée dans RUDI.

*(Ce document est en cours de rédaction)*

#
# **2. Modèlisation de la sécurité dans RUDI**

## **2.1. Concepts**

### **2.1.1. Entités**
Une Entité est une organization dans RUDI, qui peut être soit un nœud Producteur de données, soit nœud Consommateur.
Exemple de nœud Consommateur: une organisation "Porteuse de projet" qui souhaite collecter des données de façon récurrent pour proposer un service par le biais d'une application)
[TODO: à valider — les utilisateurs lambdas peuvent-ils demander un droit d'accès ou une QoS particuliers ?] 
Les utilisateurs qui dans RUDI sont référencés mais n'appartiennent pas à une organisation particulière sont considérés comme appartenant à l'Entité "Default Entity"

### **2.1.2. Groupes**
Une Entité Consommatrice qui souhaite bénéficier d'un ou plusieurs accès particuliers auprès d'une Entité Productrice de données va définir chez celui-ci autant de Groupes. 
Ex : 
- groupe "Webservices"
- groupe "Aministrateurs Consommateurs" 
- groupe "Backoffice Consommateur"

Les utilisateurs qui dans RUDI sont référencés mais n'appartiennent pas à une organisation particulière sont considérés comme appartenant au groupe "Default Group"

### **2.1.3. Utilisateurs**
Un compte utilisateur est associé soit à une personne physique, soit un robot (ex: agent de requête)
Les utilisateurs (physiques ou virtuels) d'une même Entité Consommatrice sont rassemblés au sein de différents Groupes en fonction de leur accès aux données

### **2.1.4. Rôle**

### **2.1.5. Profil d'accès**

