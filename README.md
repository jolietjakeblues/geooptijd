# Termennetwerk Geo-Temporeel Model (Places in Time)

Dit project bevat een **lichtgewicht RDF/TRiG-profiel** voor geo-temporele modellering in het kader van het Termennetwerk, geïnspireerd door het idee van **“places in time”** en het **Linked Places Format (LPF)**.

Doel: erfgoedinstellingen in staat stellen om erfgoedobjecten te koppelen aan **plaatsen-in-de-tijd** (bijv. *Weesp (gemeente, 1812–1966)*), inclusief:
- bestuurlijk type (gemeente, departement, provincie, …),
- geldigheid in de tijd,
- hiërarchie (deel van provincie/land),
- geometrie (polygon/point),
- bron en provenance.

## Bestanden

- `geotemporeel-model.trig`  
  Bevat:
  - een **schema-graph** met klassen en eigenschappen (tn:Place, tn:PlaceTimeSlice, etc.);
  - een **voorbeeld-graph** met o.a. *Weesp (gemeente, 1812–1966)* en een erfgoedobject dat eraan gekoppeld is.

Je kunt deze repository zo inladen in een triplestore (Fuseki, GraphDB, Blazegraph, …) of gebruiken als referentie voor eigen implementatie.

## Namespaces

In het model worden de volgende prefixen gebruikt:

```turtle
@prefix tn:    <https://termennetwerk.nl/def/geotemporeel#> .
@prefix ex:    <https://termennetwerk.nl/id/> .
@prefix skos:  <http://www.w3.org/2004/02/skos/core#> .
@prefix rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix geo:   <http://www.opengis.net/ont/geosparql#> .
@prefix dct:   <http://purl.org/dc/terms/> .
@prefix xsd:   <http://www.w3.org/2001/XMLSchema#> .
@prefix prov:  <http://www.w3.org/ns/prov#> .
```

## Kernklassen

- `tn:Place`  
  Abstracte plaats (continuïteit van de plaats in de tijd), subklasse van `skos:Concept`.  
  Voorbeeld: *Weesp*, *Limburg*, *Noord-Holland*.

- `tn:PlaceTimeSlice`  
  Tijdsnede van een plaats: een bestuurlijke of functionele toestand van een plaats met een geldigheidsinterval.  
  Voorbeeld: *Weesp (gemeente, 1812–1966)*, *Limburg (departement, Franse tijd)*.

- `tn:Geometry`  
  Ruimtelijke representatie (WKT, etc.) van een tijdsnede.

- `tn:Period`  
  Historische periode (optioneel), zoals *Franse tijd*, *moderne tijd*.

## Belangrijkste eigenschappen

- `tn:hasTimeSlice` – koppelt een `tn:Place` aan één of meerdere `tn:PlaceTimeSlice`-instanties.  
- `tn:ofPlace` – inverse relatie: tijdsnede → plaats.  
- `tn:validFrom` / `tn:validTo` – geldigheidsinterval van de tijdsnede.  
- `tn:adminType` – bestuurlijk type (gemeente, provincie, departement, …).  
- `tn:partOf` – hiërarchische relatie tussen tijdsnedes (bijv. gemeente ⊂ provincie).  
- `tn:hasGeometry` – koppelt een tijdsnede aan een geometrie.  
- `tn:hasPeriod` – koppelt een tijdsnede aan een historische periode.  
- `tn:locatedIn` – koppelt een erfgoedobject aan een `tn:PlaceTimeSlice`.  

### Bron en provenance

Voor bron- en herkomstinformatie worden beproefde vocabularia hergebruikt:

- `dct:source` – inhoudelijke bron (dataset, publicatie, kaart).  
- `dct:provenance` – tekstuele beschrijving van de herkomst/geschiedenis.  
- `prov:wasDerivedFrom` – formele verwijzing naar een dataset of record waar de data van is afgeleid.  

Alle drie zijn gespecificeerd als subeigenschappen (of specialisaties) van een generiek `tn:source`.

## Voorbeeld: Weesp (gemeente, 1812–1966)

In de `geotemporeel-model.trig`-file vind je:

- een abstracte `tn:Place` voor **Weesp**;
- een `tn:PlaceTimeSlice` voor **Weesp (gemeente, 1812–1966)**;
- een `tn:Geometry` met (fictieve) WKT-geometrie voor 1950;
- een `tn:PlaceTimeSlice` voor **Noord-Holland (provincie, 1840–heden)**;
- een erfgoedobject dat via `tn:locatedIn` gekoppeld is aan de tijdsnede van Weesp.

Dit voorbeeld kan direct gebruikt worden als referentie bij het ontwerpen van query’s (SPARQL), UI-ontwerpen, of mappings naar Linked Places Format (LPF / GeoJSON-T).

## Gebruik in een triplestore

Voorbeeld: inladen in Apache Jena Fuseki (CLI):

```bash
# Maak een dataset
tdb2.tdbloader --loc=dataset geotemporeel-model.trig
```

Daarna kun je SPARQL-query’s schrijven als:

```sparql
PREFIX tn:   <https://termennetwerk.nl/def/geotemporeel#>
PREFIX ex:   <https://termennetwerk.nl/id/>
PREFIX dct:  <http://purl.org/dc/terms/>

SELECT ?object ?title
WHERE {
  GRAPH <https://termennetwerk.nl/graph/examples> {
    ?object dct:title ?title ;
            tn:locatedIn ex:placetimeslice/weesp_gemeente_1812_1966 .
  }
}
```

## Licentie

Vul hier de gewenste licentie in, bijvoorbeeld:

- [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) voor documentatie en voorbeelden;
- [MIT](https://opensource.org/licenses/MIT) of [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) voor het model zelf.

Pas dit aan naar jullie eigen beleidskeuzes.

## Contact / bijdrage

Suggesties, issues en pull requests zijn welkom.  
Je kunt bijvoorbeeld bijdragen door:

- extra voorbeelden toe te voegen (andere gemeenten, departement Limburg, Veerle, …);
- mappings naar specifieke datasets (Topotijdreis, HISGIS, Kadaster) toe te voegen;
- uitbreidingen voor o.a. periodes, rollen, of meer formele tijdsmodellering (OWL-Time, CIDOC CRM, CRMgeo).
