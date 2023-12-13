# Argumentaire du modelet Temps

## Nom
Modélisation de l'évolution temporelle des entités géographiques

## Description

L'objectif de la thèse est de proposer une approche pour créer une base de connaissances d'adresses et de rues allant de la fin du XVIIIe siècle aux années 1950, sur la ville de Paris. Le travail ne se fait pas à un instant donné mais sur une période de temps. L'aspect temporel est donc à prendre en compte pour lier dans le temps l'ensemble des attestations de noms ou de localisation des repères (landmarks) composant une adresse donnée.

Ces attestations sont des mentions de la valeur ou de la modification d'une caractéristique d'un repère dans une source historique : elles décrivent les **états** des repères ou les **événements** qui leur arrivent. Ces états et ces événements se succèdent dans le temps et ont des rapports de cause à effet entre eux: par exemple, deux états successifs d'une rue, associés à des valeurs de nom différentes sont liés par un **événement de changement de nom**. Cet événement de changement de nom ne peut figurer dans la modélisation de l'évolution sans que cette condition de noms différents ne soit vérifiée.

Les événements qui peuvent intervenir au cours de l'existence d'un repère sont:
- apparition
- disparition
- réincarnation
- changement de valeur de propriété : nom officiel, nom d'usage, localisation (fusion, scission, prolongement)

Voir la typologie d'événements donnée dans *Kathleen Hornsby and Max J. Egenhofer. Identity-based change : a foundation for spatio-temporal knowledge representation. International Journal of Geographical Information Science, 14(3) :207–224, April 2000.*

Un repère peut être mentionné dans une source avant d'exister sur le terrain: sa création est planifiée par la mairie par exemple. Il peut être mentionné après sa disparition ("ancien hôtel X").

Les propriétés des repères mentionnées dans les sources sont:
- existence
- nom (officiel ou d'usage)
- localisation (issue de sources carto textuelles comme points de repère)

Ces propriétés sont caractérisées par:
- un temps valide
- une/des source.s qui attestent leur valeur
- leur type
- leur valeur

Le temps valide correspond à un intervalle dont les bornes correspondent à des événements.

On peut distinguer des grandeurs liées au temps :
* un instant : durée très courte (tendant vers 0) qui peut être décrite par une valeur de temps avec un niveau de granularité défini (~précision). On peut utiliser un instant pour décrire un événement, indiquer le moment durant lequel il se produit.
* un intervalle de temps : il correspond à une durée plus ou moins longue définie (si possible) par deux instants qui indiquent le début et la fin. On peut utiliser un instant pour décrire un état, indiquer la période durant laquelle l'état valable.

Toutefois, il peut y avoir des différences de granularité entre les données que l'on souhaite créer et celles qu'on trouve dans les sources géohistoriques. Des imprécisions peuvent aussi s'y trouver. Par exemple, on trouve souvent des données précises de création de voies mais rarement de destruction.

C'est la raison pour laquelle il est important d'introduire la notion de flou. Par exemple, on peut décrire un instant sous la forme d'un intervalle avec une distribution de probabilité autour d'un instant. On peut faire de même avec un intervalle. De même, il existe des événéments non datés mais qui ont des relations avec d'autres qui le sont (X avant Y, X pendant Y) implique aussi l'utilisation de ces temps flous.

Étant donné qu'il est nécessaire de suivre l'évolution des rues et des adresses, il faut savoir quel état suit l'autre, d'où le besoin de pouvoir faire des comparaisons entre ces grandeurs temporelles. Comparer deux instants est facile (t1 < t2 si t1 existe avant t2, t1=t2 s'ils existent au même moment, t1). Concernant deux intervalles, il y a davantage de relations qui sont définies par l'algèbre des intervalles d'Allen.

### Caractéristiques attendues des grandeurs temporelles

Un instant est défini par :
* {obligatoire} valeur temporelle (date, heure...)
* {obligatoire} granularité (précision à l'année, au jour, à la seconde...)
* {obligatoire} référentiel temporel (calendrier utilisé, fuseau horaire)

Un intervalle est défini par :
* {obligatoire} un instant initial
* {obligatoire} un instant final
* {optionnel} méthode de définition de l'intervalle (on peut définir l'intervalle à partir d'un instant initial et une durée, ce qui implique la connaissance de l'instant final / on peut connaitre simplement les instants extrêmes...)
* {optionnel} relation avec un autre intervalle (algèbre de Allen)

Un intervalle flou est défini par :
* {obligatoire} un intervalle I_noy (ou instant) noyau, l'intervalle où on est certains de l'existence de l'objet (l'ensemble des instants tels que leur fonction d'appartenance vaut 1) ;
* {obligatoire} un intervalle I_deb tel que I_deb m I_noy, désigne les instants dont la fonction d'appartenance est dans ]0,1[ et situés avant I_noy ;
* {obligatoire} un intervalle I_fin tel que I_fin mi I_noy, désigne les instants dont la fonction d'appartenance est dans ]0,1[ et situés après I_noy ;

:warning: On peut ajouter des fonctions d'appartenance si on suppose qu'elles ne sont pas linéaires.

Une durée est définie (:warning: intéressant ?) par :
* {obligatoire} valeur
* {obligatoire} unité de temps (années, minute...)

## Exemples

### Instants / événements

Dans le base de données des *Dénominations des emprises des voies actuelles* (de Paris), on trouve des dates d'arrêtés de dénomination qui décrivent un événement (création /modification d'une dénomination) avec un niveau de granularité à la journée :
>*Dénomination complète minuscule;Date de l'arrété
promenade Germaine Sablon;2022-01-24
rue du Général Appert;1900-12-07
rue Leneveux;1899-08-02
rue Isabey;1867-03-02*

### Intervalles / états
Sur la page Wikipédia sur la rue Abel Laurent (https://fr.wikipedia.org/wiki/Rue_Abel-Laurent), on trouve l'information suivante :
*Cette rue a été ouverte en 1878. Elle disparait vers 1993, lors de la démolition des entrepôts de Bercy, dans le cadre de l'aménagement de la ZAC de Bercy.*
Cette phrase décrit l'état d'existence de la rue, l'intervalle a un instant de fin flou.

### Temps flou
Sur l'atlas de Verniquet : "parachevé en 1791", aucune information sur la dates de relevés. Titre en bas de la carte : "Paris de 1789 à 1798"

### Granularité
Extrait du *Dictionnaire administratif et historique des rues de Paris et de ses monuments* par Félix Lazare :
>*ALIGRE (RUE D') [...] <br/>Cette rue, ouverte en décembre 1778 [...] <br/> avait été autorisée par des lettres-patentes du 17 février 1777*
