![image](https://github.com/user-attachments/assets/9ec73684-b97a-4da3-b924-0d63871982f9)# OSLO Mobiliteit: Intelligente Toegang - implementatiemodel LBLOD (Citerra project)

Deze repository voorziet voorbeelden om voorwaarden voor toegang autoluwe zones semantisch te beschrijven.

Deze voorbeelden zijn beschreven in RDFa formaat om besluiten op een machine-leesbare manier te ontsluiten.

Op dit moment ligt de focus op minimale voorbeeldjes om het OSLO model [OSLO Mobiliteit: Intelligente Toegang](https://data.vlaanderen.be/doc/applicatieprofiel/mobiliteit-intelligente-toegang) af te toetsen.
Later kunnen reglementteksten uit de praktijk hieraan toevoegd worden.

## Vergunning autoluwe zone (publieke dienstverlening)

* In welke steden wordt een dienstverlening aangeboden om een vergunning voor autoluwe zone te verkrijgen?

```
prefix cpsv: <http://purl.org/vocab/cpsv#>
prefix m8g: <http://data.europa.eu/m8g/>
prefix mit: <https://data.vlaanderen.be/ns/mobiliteit-intelligente-toegang#>

select ?bestuurseenheid ?dienstverlening
where {
  ?dienstverlening a cpsv:PublicService ;
                  m8g:hasCompetentAuthority ?bestuurseenheid ;
                  mit:heeftOutputtype <http://data.vlaanderen.be/id/concept/PubliekeDienstverleningOutputCode/5ab0e9b8a3b2ca7c5e00001b> .
}
```
TODO: codelijst aanmaken met output code "vergunning autoluwe zone"

## Definitie vergunningszone

* Wat is de naam en geometrie van een zone?

Om een autoluwe zone te beschrijven, gebruiken we de klasse `Zone` en eigenschappen `naam` en `geometrie`.

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# mobiliteit: https://data.vlaanderen.be/ns/mobiliteit# eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql# m8g: http://data.europa.eu/m8g/ dct: http://purl.org/dc/terms/">
    <div typeof="mobiliteit:Zone" resource="https://data.gent.be/id/zone/x">

    1.	<span property="rdfs:label">Autoluw gebied 1</span>:
         <div property="locn:geometry" typeof="locn:Geometry" resource="https://data.gent.be/id/zone/x/geometrie/1">
          <span property="geosparql:asWKT" content="<http://www.opengis.net/def/crs/EPSG/0/31370> POINT(126306.58208223493 179948.9735279791)" datatype="geosparql:wktLiteral"></span>
        </div>
    •	Diestsestraat, tussen Margarethaplein en huisnummer 188/209 en tussen Vanden Tymplestraat en R23; 
    •	Jodenstraat; 
    •	Leopold Vanderkelenstraat, tussen de Diestsestraat en de Bondgenotenlaan; 
    •	Puttegang; 
    •	Sint-Maartenstraat, tussen de Diestsestraat en parking Sint-Maartensdal (huisnummers 1, 1A en 2 - 18); 
    •	Vaartstraat, tussen de Diestsestraat en parking Gerechtsgebouw (huisnummer 7).
    </div>
</div>
```

TODO: straten annoteren

# Nieuwe aanpak

In deze nieuwe aanpak wordt er vanuit 2 insteken vertrokken:
- welke informatie (query) moet een formulier kunnen opvragen om het formulier te kunnen opbouwen
- hoe kunnen we het reglement annoteren op zodanige manier dat de originele opbouw van de tekst behouden blijft (geen datamodel push, wat in de oude aanpak onderaan wel het geval is met AND/OR constructies)
  
## Op welke zones is de dienstverlening van toepassing?

Zones kunnen op twee manieren gekoppeld worden aan de dienstverlening (vergunning).
Enerzijds impliciet wanneer deze eenmalig bovenaan het reglement (of in bijlage) wordt beschreven, anderzijds expliciet de zones op te lijsten in de beschrijving van de dienstverlening.

### Impliciet

In een reglement worden zones typisch bovenaan eenmalig gedefinieerd, zoals definities.
Deze zones zijn dan impliciet van toepassing bij de beschrijving van de dienstverlening.
Om de zones van de dienstverlening terug te vinden, gebruiken we dus het besluit zelf waarin deze vermeld staan.

```
<head>
    <meta charset="utf-8">
    <title>REGLEMENT BETREFFENDE DE TOEGANG TOT HET VOETGANGERSGEBIED</title>
</head>
<body>

  <div vocab="http://data.vlaanderen.be/ns/besluit#" prefix="lblod: http://data.lblod.info/vocabularies/lblod/ eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# mandaat: http://data.vlaanderen.be/ns/mandaat# besluit: http://data.vlaanderen.be/ns/besluit# generiek: http://data.vlaanderen.be/ns/generiek# person: http://www.w3.org/ns/person# persoon: http://data.vlaanderen.be/ns/persoon# dct: http://purl.org/dc/terms/ skos: http://www.w3.org/2004/02/skos/core# org: http://www.w3.org/ns/org# foaf: http://xmlns.com/foaf/0.1/ ext: http://mu.semte.ch/vocabularies/ext/ besluittype: https://data.vlaanderen.be/id/concept/BesluitType/ elod: http://linkedeconomy.org/ontology# lblodBesluit: http://lblod.data.gift/vocabularies/besluit/ mobiliteit: https://data.vlaanderen.be/ns/mobiliteit#">

<div typeof="foaf:Document https://data.vlaanderen.be/id/concept/BesluitType/67378dd0-5413-474b-8996-d992ef81637a"
          resource="http://een.domein.van.leuven.be/leuven/a361ed84-4c47-4ee7-b2f9-2411a15d56ff-6">
          <div resource="http://een.domein.van.leuven.be/reglement/a361ed84-4c47-4ee7-b2f9-2411a15d56ff-6">
            <p>
Volgende straten worden voorzien van een verkeersbord F103, al dan niet met een onderbord dat de wettelijke uitzonderingen bepaalt:
            </p>
    <div resource="https://data.leuven.be/id/zone/x1"
      typeof="mobiliteit:Zone"
      property="prov:atLocation">

      <p>
        1.	Stadsdeel blauw: 
      </p>

    </div>  
  </div>
</body>
```

### Expliciet




## Vergunningszone

* Welke zones hebben een vergunning nodig?

## Algemene voorwaarden

* Welke voorwaarden (en evt. bijhorende bewijsstukken) zijn van toepassing?

## Doelgroep

* Welke doelgroepen komen in aanmerking voor een vergunning?
* Welke specifieke voorwaarden zijn er?

## Periode 

* Voor hoelang kan ik een vergunning aanvragen?

# Oude aanpak

## Vergunningszone

Om een autoluwe zone te beschrijven, gebruiken we de klasse `Zone` en eigenschappen `naam` en `geometrie`.

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# mobiliteit: https://data.vlaanderen.be/ns/mobiliteit# eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql# m8g: http://data.europa.eu/m8g/ dct: http://purl.org/dc/terms/">
    <div typeof="mobiliteit:Zone" resource="https://data.gent.be/id/zone/x">
      <span property="rdfs:label">zone X</span>.
      <div property="locn:geometry" typeof="locn:Geometry" resource="https://data.gent.be/id/zone/x/geometrie/1">
<span property="geosparql:asWKT" content="<http://www.opengis.net/def/crs/EPSG/0/31370> POINT(126306.58208223493 179948.9735279791)" datatype="geosparql:wktLiteral"></span>
      </div>
    </div>
  </div>
```

Een voorwaarde met type `zone` kan naar deze zone vervolgens verwijzen:

```
  <div typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/1">
    <span property="dct:type" value="https://data.vlaanderen.be/id/concept/voorwaarde/zone">Zone
    <div property="prov:atLocation" typeof="https://data.vlaanderen.be/ns/mobiliteit#Zone" resource="https://data.gent.be/id/zone/x"> X
  </div>
</div>
```

```mermaid
classDiagram
    autoluwe_zone_x --> autoluwe_zone_x_geometrie: geometrie (locn-geometry)
    voorwaarde_zone --> concept_zone: type (dct-type)
    voorwaarde_zone --> autoluwe_zone_x: geografischeDekking (prov-atLocation)
    note for autoluwe_zone_x "URI: https:\/\/data.gent.be\/id\/zone\/x"
    note for autoluwe_zone_x_geometrie "URI: https:\/\/data.gent.be\/id\/zone\/x\/geometrie\/1"
    note for voorwaarde_zone "URI: https:\/\/data.gent.be\/id\/voorwaarden\/1"
    note for concept_zone "URI: https:\/\/data.vlaanderen.be\/id\/concept\/voorwaarde\/zone"

    class autoluwe_zone_x {
      a Zone (mobiliteit:Zone)
      naam (rdfs:label) "autoluwezone X te Gent"
    }
    class autoluwe_zone_x_geometrie {
      a Geometrie (locn:Geometry)
      wkt (geosparql:asWKT) "<http://www.opengis.net/def/crs/EPSG/0/31370> POINT(126306.58208223493 179948.9735279791)"
    }
    class voorwaarde_zone {
      a Voorwaarde (m8g:Requirement)
    }
    class concept_zone {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Zone van de vergunning."
    }
```

Het kan wenselijk zijn om zones in een ander (GIS) systeem te beheren.
Te bekijken of [heeftMeerInfo](https://data.vlaanderen.be/doc/applicatieprofiel/slimmeraadpleegomgeving/#Stuk%3AheeftMeerInfo) voldoende is om het reglement te laten wijzen naar een pagina met meer info over de zones.

## Periode

Om te beschrijven hoelang een vergunning geldig is, gebruiken we een `Voorwaarde` met type `periode` en eigenschap `duur` (`https://schema.org/duration`). Duur wordt uitgedrukt als een interval volgens iso 8601, bv P1D 1 dag, P12M 12 maanden...	

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql# m8g: http://data.europa.eu/m8g/ schema: https://schema.org/ dct: http://purl.org/dc/terms/">
Deze vergunning is 
  <div typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/2">
    <span property="dct:type" resource="https://data.vlaanderen.be/id/concept/voorwaarde/periode"></span>
    <span property="schema:duration" value="P12M">12 maanden</span>
    geldig
  </div>
</div>
```

```mermaid
classDiagram
    voorwaarde_periode --> concept_periode: type (dct-type)
    note for voorwaarde_periode "URI: https:\/\/data.gent.be\/id\/voorwaarden\/2"
    note for concept_periode "URI: https:\/\/data.vlaanderen.be\/id\/concept\/voorwaarde\/periode"
    class voorwaarde_periode {
      a Voorwaarde (m8g:Requirement)
      duur (schema:duration) "P12M"
    }
    class concept_periode {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Duurtijd van de vergunning."
    }
```

## Extra info

### Motivatie

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql# m8g: http://data.europa.eu/m8g/ schema: https://schema.org/ dct: http://purl.org/dc/terms/">
De aanvraag moet   
  <div typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/3">
    <span property="dct:type" resource="https://data.vlaanderen.be/id/concept/voorwaarde/motivatie">gemotiveerd</span>
  </div>
worden.
</div>
```

```mermaid
classDiagram
    voorwaarde_motivatie --> concept_motivatie: type (dct-type)
    note for voorwaarde_motivatie "URI: https:\/\/data.gent.be\/id\/voorwaarden\/3"
    note for concept_motivatie "URI: https:\/\/data.vlaanderen.be\/id\/concept\/voorwaarde\/motivatie"
    class voorwaarde_motivatie {
      a Voorwaarde (m8g:Requirement)
      beschrijving (dct:description) "Motiveer je aanvraag."
    }
    class concept_motivatie {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Motivatie om de vergunning te verkrijgen."
    }
```

### Deelwagen

Voorwaarde dat het gebruik van een deelwagen nagaat.

Is een collectie van voorwaarden waarbij gekozen moet worden (OR) tussen volgende voorwaarden:
* Voorwaarde: Gebruikt geen deelwagen
* Terug een collectie van voorwaarden (AND):
  * Gebruikt wel een deelwagen
  * Bewijs deelwagen

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# prov: http://www.w3.org/ns/prov# adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql# m8g: http://data.europa.eu/m8g/ mit: https://data.vlaanderen.be/ns/mobiliteit-intelligente-toegang# schema: https://schema.org/ dct: http://purl.org/dc/terms/">
De aanvrager moet aangeven
<div typeof="mit:Voorwaardecollectie m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/4">
    <span property="dct:description">
        of een deelwagen gebruikt zal worden.
        <span property="mit:operatie" value="https://data.vlaanderen.be/id/concept/logischeOperatie/OR"></span>
        <div property="m8g:hasRequirement" typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/5">
            <span property="dct:type" resource="https://data.vlaanderen.be/id/concept/voorwaarde/gebruiktGeenDeelwagen"></span>
            <span property="dct:description>Ofwel wordt er geen deelvoertuig gebruikt.</span>
        </div>
        <div property="m8g:hasRequirement" typeof="mit:Voorwaardecollectie m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/6">
            <span property="mit:operatie" value="https://data.vlaanderen.be/id/concept/logischeOperatie/AND"></span>
            <div property="m8g:hasRequirement" typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/7">
                <span property="dct:type" resource="https://data.vlaanderen.be/id/concept/voorwaarde/gebruiktDeelwagen"></span>
                <span property="dct:description>Ofwel wordt er wel een deelvoertuig gebruikt </span>
            </div>
            <div property="m8g:hasRequirement" typeof="m8g:Requirement" resource="https://data.gent.be/id/voorwaarden/8">
                <span property="dct:type" resource="https://data.vlaanderen.be/id/concept/voorwaarde/deelwagenbewijs"></span>
                <span property="dct:description>en wordt aangetoond met een document dat de aanvrager deze deelwagen gebruikt.</span>
                <div property="m8g:hasEvidenceTypeList" typeof="m8g:EvidenceTypeList" resource="https://data.gent.be/id/bewijstypelijst/1">
                    <div property="m8g:specifiesEvidenceType" typeof="m8g:EvidenceType" resource="https://data.gent.be/id/bewijstype/1">
                        <span property="m8g:evidenceTypeClassification" value="https://data.vlaanderen.be/id/concept/bewijstypeclassificatie/deelwagenbewijs"></span>
                    </div>
                </div>
            </div>
        </div>
    </span>
</div>
```

```mermaid
classDiagram
    voorwaardecollectie_deelwagen --> concept_deelwagen: type (dct-type)
    voorwaardecollectie_deelwagen --> voorwaarde_gebruiktGeenDeelwagen: heeftVoorwaarde (m8g-hasRequirement)
    note for voorwaardecollectie_deelwagen "URI: https:\/\/data.gent.be\/id\/voorwaarden\/4"
    note for concept_deelwagen "URI: https:\/\/data.vlaanderen.be\/id\/concept\/voorwaarde\/deelwagen"
    class voorwaardecollectie_deelwagen {
      a Voorwaardecollectie (mit:Voorwaardecollectie)
      beschrijving (dct:description) "of een deelwagen gebruikt zal worden. Ofwel wordt er geen deelvoertuig gebruikt. Ofwel wordt er wel een deelvoertuig gebruikt en wordt aangetoond met een document dat de aanvrager deze deelwagen gebruikt."
      operatie (mit:operatie) "OR"
    }
    class concept_deelwagen {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Al dan niet gebruik van deelwagen."
    }
    note for voorwaarde_gebruiktGeenDeelwagen "URI: https:\/\/data.gent.be\/id\/voorwaarden\/5"
    voorwaarde_gebruiktGeenDeelwagen --> concept_gebruiktGeenDeelwagen: type (dct-type)
    class voorwaarde_gebruiktGeenDeelwagen {
        a Voorwaarde (m8g:Requirement)
        beschrijving (dct:description) "Ofwel wordt er geen deelvoertuig gebruikt."
    }
    class concept_gebruiktGeenDeelwagen {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Aanvrager gebruikt geen deelwagen."
    }

    note for voorwaardecollectie_gebruiktDeelwagen "URI: https:\/\/data.gent.be\/id\/voorwaarden\/6"
    class voorwaardecollectie_gebruiktDeelwagen {
      a Voorwaardecollectie (mit:Voorwaardecollectie)
      beschrijving (dct:description) "Ofwel wordt er wel een deelvoertuig gebruikt en wordt aangetoond met een document dat de aanvrager deze deelwagen gebruikt."
      operatie (mit:operatie) "AND"
    }
    voorwaardecollectie_deelwagen --> voorwaardecollectie_gebruiktDeelwagen: heeftVoorwaarde (m8g-hasRequirement)

    voorwaardecollectie_gebruiktDeelwagen --> voorwaarde_gebruiktDeelwagen: heeftVoorwaarde (m8g-hasRequirement)
    note for voorwaarde_gebruiktDeelwagen "URI: https:\/\/data.gent.be\/id\/voorwaarden\/7"
    voorwaarde_gebruiktDeelwagen --> concept_gebruiktDeelwagen: type (dct-type)
    class voorwaarde_gebruiktDeelwagen {
        a Voorwaarde (m8g:Requirement)
        beschrijving (dct:description) "Ofwel wordt er wel een deelvoertuig gebruikt "
    }
    class concept_gebruiktDeelwagen {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Aanvrager gebruikt deelwagen."
    }

    voorwaardecollectie_gebruiktDeelwagen --> voorwaarde_deelwagenbewijs: heeftVoorwaarde (m8g-hasRequirement)

    note for voorwaarde_deelwagenbewijs "URI: https:\/\/data.gent.be\/id\/voorwaarden\/8"
    voorwaarde_deelwagenbewijs --> concept_deelwagenbewijs: type (dct-type)
    class voorwaarde_deelwagenbewijs {
        a Voorwaarde (m8g:Requirement)
        beschrijving (dct:description) "wordt aangetoond met een document dat de aanvrager deze deelwagen gebruikt."
    }
    class concept_deelwagenbewijs {
      a Concept (skos:Concept)
      label (skos:prefLabel) "Aanvrager toont gebruik van deelwagen aan met document."
    }
    voorwaarde_deelwagenbewijs --> bewijstypelijst_deelwagenbewijs: heeftBewijstypelijst (m8g-hasEvidenceTypeList)
    note for bewijstypelijst_deelwagenbewijs "URI: https:\/\/data.gent.be\/id\/bewijsttypelijst\/1"
    class bewijstypelijst_deelwagenbewijs {
        a Bewijstypelijst (m8g:EvidenceTypeList)
    }
    bewijstypelijst_deelwagenbewijs --> bewijstype_deelwagen: specifieertBewijstype (m8g-specifiesEvidenceType)
    note for bewijstype_deelwagen "URI: https:\/\/data.gent.be\/id\/bewijstype\/1"
    class bewijstype_deelwagen {
        a Bewijstype (m8g:EvidenceType)
    }
```

TODO model:
* type niet verplichten op Voorwaardecollectie
