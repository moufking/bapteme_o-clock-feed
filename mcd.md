 ### #Retour 
 
Concernant le diagramme il y'a des erreurs concernant les cardinalités 

En effet 

 - Un utiilisateur peut créer sur son compte un ou plusieurs addresses de livraison donc les cardinalités sont:
1, N au lieu de 0, N du coté des utilisateurs et 1, 1 du coté de l'entité ADDRESS

- Un utilisateur peut aimer un ou plusieurs produits. Par conséquent, nous devrions avoir une relation 0,N du côté de l'entité User et une relation 1,N du côté de l'entité Product.
  
- La relation entre l'entité ORDER et USER est exacte : car un utilisateur peut faire une ou plusieurs commandes, d'où une relation 1,N du côté de l'entité User, et à une commande on associe un seul utilisateur, d'où une relation 1,1 du côté de l'entité Order.
  
- Pour répresenter les diagrammes on dispose de divers outils comme :

   - Lucidchart : Il s'agit d'un outil de conception de diagrammes en ligne qui prend en charge la création de diagrammes UML, ainsi que d'autres types de diagrammes.
   - Draw.io : Cet outil en ligne gratuit permet de créer des diagrammes UML, ainsi que d'autres types de diagrammes.
     Creately : Il s'agit d'un outil de conception de diagrammes en ligne qui propose des modèles prédéfinis pour créer des diagrammes UML.

   - PlantUML : Il s'agit d'un générateur de diagrammes UML basé sur du code, qui permet de créer des diagrammes UML à l'aide de textes simples.

   - Visual Paradigm : C'est un outil de modélisation UML complet avec une interface graphique intuitive et des fonctionnalités avancées de collaboration et de partage de projets.

   - Gliffy : Cet outil en ligne permet de créer des diagrammes UML et d'autres types de diagrammes avec une interface simple et intuitive.
    



  

