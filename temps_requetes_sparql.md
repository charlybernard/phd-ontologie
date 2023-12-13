# Requêtes SPARQL pour le modelet Temps

Requêtes SPARQL décrivant les questions informelles de compétence

## Pour connaître les entités géographiques d'un type défini qui existent à un instant donné

Quelles sont les voies existant à Paris en 1860 ? Note : `<http://www.wikidata.org/entity/Q1985727>` indique que le calendrier dans lequel est inscrit la date est le calendrier grégorien proleptique.

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
