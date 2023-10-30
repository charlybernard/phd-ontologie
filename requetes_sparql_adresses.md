# Requêtes SPARQL pour le modelet Adresses

Requêtes SPARQL décrivant les questions informelles de compétence

## Pour connaître l'ensemble des adresses situées le long d'une rue définie par un libellé

Quelles sont les adresses situées le long de la rue d'Anjou ?
```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX geo: <http://www.opengis.net/ont/geosparql#>
SELECT ?address ?addressLabel ?geom ?streetName WHERE {
    ?lm a addr:Landmark; addr:isLandmarkType addr:Thoroughfare; rdfs:label ?streetName.
    FILTER(LCASE(STR(?streetName)) = LCASE("rue d'Anjou") && LANG(?streetName) = "fr")
    ?address a addr:Address; rdfs:label ?addressLabel.
    OPTIONAL {?address addr:targets [geo:asWKT ?geom].}
    {?address addr:firstStep ?addrSegment}UNION{?address addr:firstStep [addr:nextStep+ ?addrSegment]}
    ?addrSegment a addr:AddressSegment; addr:relatum ?lm.
}
```

## Pour connaître les coordonnées d'une adresse selon son libellé

Quelles sont les coordonnées de l'adresse dont le libellé est "41 rue de Rivoli, 75001 Paris 1er arrondissement" ?

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX gsp: <http://www.opengis.net/ont/geosparql#>

SELECT ?item ?addressLabel ?coords WHERE {
    ?item a addr:Address;
          rdfs:label ?addressLabel; 
          addr:targets [gsp:asWKT ?coords].
    FILTER(LCASE(STR(?addressLabel)) = LCASE("41 rue de Rivoli, 75001 Paris 1er arrondissement") && LANG(?addressLabel) = "fr")
}
```

## Pour connaître la liste des adresses situées au sein d'une zone géographique donnée

Quelles sont les adresses située dans sur les îles centrales de Paris ?

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX gsp: <http://www.opengis.net/ont/geosparql#>
PREFIX gspf: <http://www.opengis.net/def/function/geosparql/>

SELECT ?s ?nb ?streetName ?coords WHERE {
    ?s a addr:Address; addr:targets [rdfs:label ?nb;
                       addr:isPartOf [addr:isLandmarkType addr:Thoroughfare; rdfs:label ?streetName];
                                                                              gsp:asWKT ?coords].
        FILTER (
        gspf:sfWithin(
            ?coords,
            "<http://www.opengis.net/def/crs/OGC/1.3/CRS84> POLYGON ((2.338886 48.858207, 2.345817 48.857007, 2.352061 48.854847, 2.359893 48.851727, 2.360559 48.851458, 2.360601 48.850061, 2.360601 48.849114, 2.356503 48.850216, 2.352576 48.851388, 2.349722 48.852094, 2.348199 48.852743, 2.344787 48.854042, 2.342191 48.855285, 2.33968 48.857134, 2.338328 48.857882, 2.338886 48.858207))"^^gsp:wktLiteral
        )
    )
}
```
