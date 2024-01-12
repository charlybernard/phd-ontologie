# Ontologie(s) des adresses
Répertoire contenant les fichiers produits pour l'ontologie de modélisation des adresses.

# Structure

Chaque modelet est divisé en trois fichiers : 
* l'argumentaire dont le nom est `{nom_du_modelet}_argumentaire.md` qui décrit le scénario de motivation du modelet ;
* le glossaire dont le nom est `{nom_du_modelet}_glossaire.md` qui regroupe l'ensemble des termes utilisés avec leur définition ; 
* les questions de compétence dont le nom est `{nom_du_modelet}_questions.md` qui donne les questions de compétence du modelet ;
* les requêtes SPARQL dont le nom est `{nom_du_modelet}_requetes_sparql.md` qui donne les transcriptions des questions de compétence du modelet en requêtes SPARQL ;
* l'ontologie dont le nom est `{nom_du_modelet}_ontology.md` qui l'ontologie associée au modelet.

# Contenu

Les modelets définis ici sont :
* `adresses` : représentation des adresses comme étant des énoncés structurés d'un cheminement à l’intérieur d’une hiérarchie spatiale, non ambigu au sein de cette hiérarchie, composé d’une suite ordonnée de repères spatiaux dont la conceptualisation et la désignation sont connues et partagées ;
* temps : représentation de l'évolution temporelle d'entités spatiales.
