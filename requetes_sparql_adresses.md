# Requêtes SPARQL pour le modelet Adresses

Requêtes SPARQL décrivant les questions informelles de compétence

## Pour connaître l'ensemble des adresses situées le long d'une rue définie par un libellé

Quelles sont les adresses situées le long de la rue d'Anjou ?
```sqarql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX geo: <http://www.opengis.net/ont/geosparql#>
SELECT ?address ?addressLabel ?geom WHERE {{
    ?lm a addr:Landmark; addr:isLandmarkType addr:Thoroughfare; rdfs:label ?streetName.
    FILTER(LCASE(STR(?streetName)) = "rue d'Anjou" && LANG(?streetName) = "fr")
    ?address a addr:Address; rdfs:label ?addressLabel.
    OPTIONAL {?address addr:targets [geo:asWKT ?geom].}
    {?address addr:firstStep ?addrSegment}UNION{?address addr:firstStep [addr:nextStep+ ?addrSegment]}
    ?addrSegment a addr:AddressSegment; addr:relatum ?lm.
}
```
