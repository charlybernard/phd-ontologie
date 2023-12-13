# Questions informelles de compétence du modelet Temps

## Question 1

### Question
Quels sont les entités géographiques d'un type défini qui existent à un instant donné ?

### Résultat attendu 
La liste des entités géographiques selon le type donné (voie, quartier...) avec leur nom d'époque.

### Exemple
Question : Quelles sont les voies existant à Paris en 1860 ?<br>
Réponse :<br>
> addr:LM__00534107-721b-49b6-b126-98b5fe0681a6,"rue de Rochechouart"@fr<br>
> ...<br>
> addr:LM__99c2ec65-89d1-404e-93a1-6b1cbd68ed5a,"place du Trône"@fr<br>

## Question 2

### Question
Sur quel(s) intervalle(s) temporel(s) est valable une adresse de dénomination donnée ?

### Résultat attendu
Une liste d'intervalles temporelles donnant la validité de l'adresse.

### Exemple
Question : En quelles années peut-on trouver l'adresse dont le libellé est "3 rue de la vieille place aux veaux" ?<br>
Réponse : les années où cette adresse existe sous la dénomination "3 rue de la vieille place aux veaux", représentées sour la forme d'un (ou plusieurs) intervalle(s) temporel(s) : `[1646-1855]`

## Question 3

### Question
Quel est l’historique d’un repère ?<br>
Deux alternatives existent: 
* (a) quels sont les événements qui lui sont associés ?
* (b) quels sont les états qui lui sont associés ?

### Résultat attendu 
Une liste des événements décrivant les changements du repère (a) ou une liste des versions ordonnées décrivant chacune le repère sur un intervalle temporel donné (b).

### Exemple
Question : Quel est l'historique de la place de la Nation ?<br>
Réponses :<br>
(a)
> 1728 : création de la place en place du Trône<br>
> 10 août 1792 : changement de nom , *place du Trône* devient *place du Trône Renversé*<br>
> <br>
> 2 juillet 1880 : changement de nom , *place du Trône* devient *place de la Nation*<br>

(b)
> * [1728,1792-08-10] : <br>
>   * nom : place du Trône<br>
>   * geométrie : "POLYGON(...)"<br>
>   * localisation : ancien 8e arrondissement de Paris<br>
> * ...<br>
> * [1880-07-02,...] :<br>
>   * nom : place de la Nation<br>
>   * géométrie : "POLYGON(...)"<br>
>   * localisation : 11e et 12e arrondissement de Paris

## Question 4

### Question
Quels sont les états et les événements qui manquent dans l'historique d'une adresse ?

### Résultat attendu 
Une liste d'états (respectivement d'évènements) "fantômes" ainsi que les évènements (respectivement les états) auxquels ils sont liés.
