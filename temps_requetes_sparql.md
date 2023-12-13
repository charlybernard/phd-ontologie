# Requêtes SPARQL pour le modelet Temps

Requêtes SPARQL décrivant les questions informelles de compétence

## Pour connaître les entités géographiques d'un type défini qui existent à un instant donné

Quelles sont les voies existant à Paris en 1860 ? Note : `<http://www.wikidata.org/entity/Q1985727>` indique que le calendrier dans lequel est inscrite la date est le calendrier grégorien proleptique.

```sparql
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?street ?name WHERE {
  BIND("1860-01-08T00:00:00"^^xsd:dateTime AS ?timeValueRef)
  BIND(<http://www.wikidata.org/entity/Q1985727> AS ?timeCalendarRef)
  ?street a addr:Landmark; addr:isLandmarkType addr:Thoroughfare; addr:hasAttribute ?attrName.
  ?changeStreet1 addr:dependsOn ?eventStreet1; addr:isChangeType addr:Dissolution; addr:appliedTo ?street.
  ?changeStreet2 addr:dependsOn ?eventStreet2; addr:isChangeType addr:Creation; addr:appliedTo ?street.
  ?eventStreet1 a addr:Event.
  ?eventStreet2 a addr:Event.
  ?attrName a addr:Attribute; addr:isAttributeType addr:NameAttribute; addr:version ?attrNameVersion.
  ?attrNameVersion a addr:AttributeVersion; addr:value ?name.
  ?change1 addr:before ?attrNameVersion; addr:dependsOn ?event1.
  ?change2 addr:after ?attrNameVersion; addr:dependsOn ?event2.
  ?event1 a addr:Event.
  ?event2 a addr:Event.
  OPTIONAL {?event1 addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValue1; addr:timePrecision ?timePrecision1; addr:timeCalendar ?timeCalendar1]}
  OPTIONAL {?event2 addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValue2; addr:timePrecision ?timePrecision2; addr:timeCalendar ?timeCalendar2]}
  OPTIONAL {?eventStreet1 addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValueStreet1; addr:timePrecision ?timePrecisionStreet1; addr:timeCalendar ?timeCalendarStreet1]}
  OPTIONAL {?eventStreet2 addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValueStreet2; addr:timePrecision ?timePrecisionStreet2; addr:timeCalendar ?timeCalendarStreet2]}
  FILTER((
      (?timeValue1 >= ?timeValueRef && ?timeValue2 <= ?timeValueRef) ||
      (?timeValue1 >= ?timeValueRef && !BOUND(?timeValue2)) ||
      (?timeValue2 <= ?timeValueRef && !BOUND(?timeValue1)) ||
      (!BOUND(?timeValue1) && !BOUND(?timeValue2))
      )
   && (
      ((?timeCalendar1 = ?timeCalendarRef) && (?timeCalendar2 = ?timeCalendarRef)) ||
      ((?timeCalendar1 = ?timeCalendarRef) && (!BOUND(?timeCalendar2))) ||
      ((?timeCalendar2 = ?timeCalendarRef) && (!BOUND(?timeCalendar1))) ||
      (!BOUND(?timeCalendar1) && !BOUND(?timeCalendar2))
      ))
  FILTER((
      (?timeValueStreet1 >= ?timeValueRef && ?timeValueStreet2 <= ?timeValueRef) ||
      (?timeValueStreet1 >= ?timeValueRef && !BOUND(?timeValueStreet2)) ||
      (?timeValueStreet2 <= ?timeValueRef && !BOUND(?timeValueStreet1)) ||
      (!BOUND(?timeValueStreet1) && !BOUND(?timeValueStreet2))
      )
   && (
      ((?timeCalendarStreet1 = ?timeCalendarRef) && (?timeCalendarStreet2 = ?timeCalendarRef)) ||
      ((?timeCalendarStreet1 = ?timeCalendarRef) && (!BOUND(?timeCalendarStreet2))) ||
      ((?timeCalendarStreet2 = ?timeCalendarRef) && (!BOUND(?timeCalendarStreet1))) ||
      (!BOUND(?timeCalendarStreet1) && !BOUND(?timeCalendarStreet2))
      ))
}
```

## Pour connaître les intervalles temporels de validité d'une adresse selon une dénomination donnée

En quelles années peut-on trouver l'adresse dont le libellé est "3 rue de la vieille place aux veaux" ?

```sparql
```

## Pour obtenir l'historique d'un repère

Quels sont les événements liés à la place de la Nation concernant un changement de nom ?

```sparql
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT ?street ?change ?event ?timeValue ?timePrecision ?timeCalendar ?beforeVersion ?afterVersion WHERE { 
    ?street a addr:Landmark; addr:isLandmarkType addr:Thoroughfare; addr:hasAttribute ?attrName; (rdfs:label|skos:altLabel) "place de la Nation"@fr.
    ?attrName a addr:Attribute; addr:isAttributeType addr:NameAttribute.
    ?change addr:appliedTo ?attrName; addr:dependsOn ?event.
    ?event a addr:Event.
    OPTIONAL {?event addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValue; addr:timePrecision ?timePrecision; addr:timeCalendar ?timeCalendar]}
    OPTIONAL {?change addr:before [addr:value ?beforeVersion]}
    OPTIONAL {?change addr:after [addr:value ?afterVersion]}
}
```

Quels est l'historique des noms de la place de la Nation ? (ensemble des noms avec leur intervalle de validité)

```sparql
PREFIX addr: <http://rdf.geohistoricaldata.org/address#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT ?street ?name ?timeValueStart ?timeValueEnd WHERE { 
    ?street a addr:Landmark; addr:isLandmarkType addr:Thoroughfare; addr:hasAttribute ?attrName; (rdfs:label|skos:altLabel) "place de la Nation"@fr.
    ?attrName a addr:Attribute; addr:isAttributeType addr:NameAttribute; addr:version ?attrNameVersion.
    ?attrNameVersion addr:value ?name.
    ?changeEnd addr:before ?attrNameVersion; addr:appliedTo ?attrName; addr:dependsOn ?eventEnd.
    ?changeStart addr:after ?attrNameVersion; addr:appliedTo ?attrName; addr:dependsOn ?eventStart.
    ?eventEnd a addr:Event.
    ?eventStart a addr:Event.
    OPTIONAL {?eventEnd addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValueEnd; addr:timePrecision ?timePrecisionEnd; addr:timeCalendar ?timeCalendarEnd]}
    OPTIONAL {?eventStart addr:hasTime [a addr:TimeInstant; addr:timeStamp ?timeValueStart; addr:timePrecision ?timePrecisionStart; addr:timeCalendar ?timeCalendarStart]}
}
```
