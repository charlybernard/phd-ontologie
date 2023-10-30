# Questions informelles de compétence du modelet Adresses

## Question 1

### Question
Connaître la liste des adresses situées le long d'une rue, dans un quartier donné...

### Résultat attendu 
La liste d'adresses distinctes associées au repère donné : numéro + rue + section si numérotation sectionnaire.

### Exemple
Question : Quelles sont les adresses répertoriées rue d'Anjou ?\
Réponse :
> 1 rue d'Anjou, 75008 Paris\
> ...\
> 80 rue d'Anjou, 75008 Paris

## Question 2

### Question
Connaître les coordonnées d'une adresse donnée par son libellé

### Résultat attendu
Les coordonnées correspondant à l'adresse, exprimées dans un système de référence de coordonnées.

### Exemple
Question : Quelles sont les coordonnées de l'adresse "2 Rue du chat qui pêche" en 1821?\
Réponse : 48.853046 , 2.34608 (en WGS84)

## Question 3

### Question
Connaître les adresses localisées dans la zone d'emprise Xmin, Xmax, Ymin, Ymax.

### Résultat attendu
La liste des adresses dont les coordonnées sont situées dans l'emprise fournie.

### Exemple
Question : quelles sont les adresses situées dans la zone définie par `POLYGON((2.34707 48.85858, 2.35044 48.85858, 2.35044 48.85689, 2.34707 48.85689,2.34707 48.85858))`\
Réponse :
> 41 rue de Rivoli, Paris / POINT(2.34792 48.85850)\
> ...\
> 8 rue Saint-Martin, Paris / POINT(2.34956 48.85759)
