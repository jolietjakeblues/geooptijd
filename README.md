# Linked Places Ontologie (LPO) – Erfgeo Datasetmodel

![License](https://img.shields.io/badge/license-CC--BY--4.0-blue)
![RDF](https://img.shields.io/badge/format-RDF%2FJSON--LD-orange)
![SHACL](https://img.shields.io/badge/validation-SHACL-green)

---

## Overzicht

Dit project bevat de **Linked Places Ontologie (LPO)** en bijbehorende **datasetprofielen**, **SPARQL-constructies**, en **validatievormen (SHACL)**  
zoals toegepast binnen het project **Erfgeo** van de Rijksdienst voor het Cultureel Erfgoed (RCE).

Het doel van deze repository is om historische en ruimtelijke entiteiten (zoals **gemeenten**, **departementen**, **waterschappen** en **parochies**) eenduidig te modelleren en publiceren als **Linked Data** die compatibel is met de [Linked Pasts](https://linkedpasts.org/) en [Linked Places](https://github.com/LinkedPasts/linked-places) standaarden.

---

## Inhoud

| Bestand | Beschrijving |
|----------|---------------|
| [`lpo-styleguide.md`](./lpo-styleguide.md) | De volledige style guide met modelprincipes, URI-patronen, en SPARQL-constructvoorbeelden. |
| [`lpo-shapes.ttl`](./lpo-shapes.ttl) | SHACL-validatievormen voor `lpo:Place`, `lpo:PlaceName`, `lpo:When`, `lpo:PlaceGeom`, enz. |
| [`ontology/lpo.ttl`](./ontology/lpo.ttl) | De formele OWL/Turtle-ontologie voor alle LPO-klassen en -eigenschappen. |
| [`context/lpo-context.jsonld`](./context/lpo-context.jsonld) | De JSON-LD `@context` die de korte veldnamen in `examples/` koppelt aan de LPO-termen. |
| [`examples/`](./examples/) | Voorbeelddata in JSON-LD op basis van echte erfgeo-datasets. |
| [`LPO_1.1_structuur.md`](./LPO_1.1_structuur.md) | Achtergronddocumentatie over de oorspronkelijke Linked Pasts Ontology v1.1 waarop dit profiel voortbouwt. |

> De Graphviz-diagrammen staan niet als losse bestanden in dit project, maar als DOT-code ingebed in `lpo-styleguide.md` (zie [Diagrammen](#diagrammen) hieronder).

---

## Ontologie (samenvatting)

**Namespace:**  
`https://linkeddata.cultureelerfgoed.nl/def/lpo#`

**Belangrijkste klassen:**

- `lpo:Place` – kernentiteit  
- `lpo:PlaceName` – naamattestaties  
- `lpo:PlaceType` – typologie (gemeente, departement, abdij, …)  
- `lpo:PlaceGeom` – ruimtelijke representatie  
- `lpo:When` – tijdsinterval  
- `lpo:Citation` – bronverwijzing  
- `lpo:SourceLabel` – labelinformatie van types  
- `lpo:PlaceRelated` – relaties tussen plaatsen  
- `lpo:PlaceDescription` – tekstuele toelichting  
- `lpo:Period` – historische perioden  

De ontologie volgt het RDF/OWL-principe van minimale afhankelijkheden en is **volledig JSON-LD compatibel**.

---

## URI-structuur

| Type | Patroon | Voorbeeld |
|------|----------|-----------|
| `Place` | `https://linkeddata.cultureelerfgoed.nl/erfgeo/{dataset}/id/{localId}` | `.../gemeenten/id/s-Hertogenbosch-1812` |
| `Geom` | `{PlaceURI}/geom/{n}` | `.../id/s-Hertogenbosch-1812/geom/1` |
| `When` | `{PlaceURI}/when/{n}` | `.../id/s-Hertogenbosch-1812/when/1` |
| `Name` | `{PlaceURI}/name/pref` of `/name/alt` | `.../id/s-Hertogenbosch-1812/name/pref` |
| `Type` | `{PlaceURI}/type/{term}` | `.../id/s-Hertogenbosch-1812/type/gemeente` |

> Alle URI’s zijn bedoeld om **resolvabel** te zijn (HTML voor mensen, RDF voor machines).

---

## Werkwijze

1. **Brondata inladen** in GraphDB (of andere triplestore).  
2. **SPARQL CONSTRUCT** uitvoeren om data naar LPO-model te transformeren.  
   - Zie de voorbeelden in de style guide (`lpo-styleguide.md`).  
3. **Validatie uitvoeren** via SHACL:
   - Laad `lpo-shapes.ttl` als shapes graph.  
   - Gebruik GraphDB’s “SHACL Validation” functie.  
4. **Exporteren** als JSON-LD (met gemeenschappelijke `@context`).  
   - Gebruik de context:  
     [`https://linkeddata.cultureelerfgoed.nl/context/lpo-context.jsonld`](https://linkeddata.cultureelerfgoed.nl/context/lpo-context.jsonld)  

---

## Validatie

SHACL-validatie volgt de shapes in [`lpo-shapes.ttl`](./lpo-shapes.ttl):

- `lposh:PlaceShape`  
- `lposh:PlaceNameShape`  
- `lposh:PlaceGeomShape`  
- `lposh:WhenShape`  
- `lposh:PlaceTypeShape`  
- `lposh:CitationShape`

### Voorbeeld GraphDB-validatie

1. Ga naar **“SHACL Validation”** in GraphDB.  
2. Kies je datasetgraph als **data graph**.  
3. Kies `lpo-shapes.ttl` als **shapes graph**.  
4. Klik **Validate** → rapport toont ontbrekende properties of foutieve types.

---

## Diagrammen

De visuele representatie van het model staat als Graphviz DOT-code in [`lpo-styleguide.md`](./lpo-styleguide.md#4-conceptueel-diagram-graphviz). Plak die code in [GraphvizOnline](https://dreampuf.github.io/GraphvizOnline/) om het model te bekijken, of render het lokaal naar SVG of PNG:

```bash
dot -Tsvg lpo_model.dot -o lpo_model.svg
```
