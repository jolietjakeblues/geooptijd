---
title: LPO Style Guide
sidebar_label: LPO Style Guide
---

# LPO Style Guide  
_Linked Places Ontologie (LPO) – Erfgeo Datasetmodel_

**Versie:** 1.0  
**Datum:** 2025-11-04  
**Ontologie-IRI:** <https://example.org/lpo#>

---

## 🎯 Doel

Deze gids beschrijft hoe erfgeo-datasets (zoals **Gemeentegeschiedenis**, **RCE departementen**, enz.) gemodelleerd worden met de LPO-ontologie  
(<https://example.org/lpo#>).

Doelen:

- semantisch consistente RDF-data;
- interoperabiliteit met **Linked Pasts / Linked Places**;
- JSON-LD / GeoJSON-compatibele output;
- hergebruik in viewers en tijd-ruimte-analyses.

---

## 🧩 1. Kernprincipes

| # | Principe |
|---|-----------|
| 1 | **Eén `lpo:Place` per entiteit** (gemeente, departement, waterschap, parochie, …), met een stabiele URI. |
| 2 | Gebruik **URI’s** voor herbruikbare entiteiten: `lpo:Place`, `lpo:PlaceType`, `lpo:Period`, etc. |
| 3 | Gebruik **blank nodes** voor lokale structuren: `lpo:PlaceName`, `lpo:When`, `lpo:PlaceGeom`, `lpo:Citation`. |
| 4 | Modelleer **taal** (`lpo:language`) en **tijd** (`lpo:When`) expliciet. |
| 5 | Leg **herkomst** altijd vast: `lpo:sourceId`, `lpo:sourceDataset`, `lpo:sourceDatasetURI`. |
| 6 | Zet geometrie als **WKT in EPSG:4326** (WGS84): `lpo:wktValue` (optioneel ook `geo:asWKT`). |
| 7 | Zorg dat RDF direct **JSON-LD-ready** is via een gemeenschappelijke `@context`. |

---

## 🧱 2. URI-beleid

### 2.1 URI-patronen

| Type | Patroon | Voorbeeld |
|------|---------|-----------|
| **Place** | `https://linkeddata.cultureelerfgoed.nl/erfgeo/{dataset}/id/{localId}` | `…/erfgeo/gemeenten/id/s-Hertogenbosch-1812` |
| **Type** | `https://linkeddata.cultureelerfgoed.nl/erfgeo/{dataset}/type/{typeName}` | `…/erfgeo/gemeenten/type/gemeente` |
| **Geom** | `{PlaceURI}/geom/{n}` | `…/id/s-Hertogenbosch-1812/geom/1` |
| **When** | `{PlaceURI}/when/{n}` | `…/id/s-Hertogenbosch-1812/when/1` |
| **Relation** | `{PlaceURI}/relation/{n}` | `…/id/s-Hertogenbosch-1812/relation/1` |

> 💡 Aanbeveling: maak URI’s resolvabel.  
> HTML voor mensen, RDF (Turtle / JSON-LD) voor machines via content-negotiation.

---

## 🏗️ 3. Klassen & eigenschappen

**Ontologie:** <https://example.org/lpo#>

| Klasse | Gebruik | Belangrijke eigenschappen |
|--------|---------|---------------------------|
| `lpo:Place` | Kernentiteit voor elke historische of moderne plaats. | `lpo:hasTitle`, `lpo:sourceId`, `lpo:sourceDataset`, `lpo:sourceDatasetURI`, `lpo:hasName`, `lpo:hasType`, `lpo:hasGeom`, `lpo:hasWhen` |
| `lpo:PlaceName` | Naamattestaties (meervoud, meertalig). | `lpo:toponym`, `lpo:language` |
| `lpo:PlaceType` | Typologie (gemeente, departement, abdij, …). | `lpo:identifier`, `lpo:typeLabel`, `lpo:hasSourceLabel` |
| `lpo:SourceLabel` | Labelinformatie van types. | `lpo:labelValue`, `lpo:labelLanguage` |
| `lpo:PlaceGeom` | Ruimtelijke representatie. | `lpo:wktValue`, `geo:asWKT`, `lpo:reprPoint`, `lpo:s2CellId`, `lpo:certainty` |
| `lpo:When` | Tijdsinterval, bestaansduur. | `lpo:start`, `lpo:end`, `lpo:earliest`, `lpo:latest`, `lpo:duration`, `lpo:sourceYear` |
| `lpo:PlaceRelated` | Relaties tussen plaatsen. | `lpo:relationType`, `lpo:relationTo`, `lpo:certainty` |
| `lpo:PlaceDescription` | Tekstuele beschrijvingen. | `lpo:descriptionValue`, `lpo:language` |
| `lpo:Citation` | Bronnen. | `lpo:citationURI`, `lpo:sourceYear` |
| `lpo:Period` | Historische perioden. | `lpo:periodName`, `lpo:periodURI` |

---

## 🧭 4. Conceptueel diagram (Graphviz)

### 4.1 Volledig model

Gebruik bijvoorbeeld [GraphvizOnline](https://dreampuf.github.io/GraphvizOnline/) en plak onderstaande DOT-code:

```dot
digraph LPO_Place_Model {
  rankdir=LR;
  node [fontname="Helvetica"];

  // ===== Klassen =====
  Place        [label="lpo:Place", shape=box, style=filled, fillcolor="#f0f0f0"];
  PlaceName    [label="lpo:PlaceName", shape=box];
  PlaceType    [label="lpo:PlaceType", shape=box];
  PlaceGeom    [label="lpo:PlaceGeom", shape=box];
  PlaceRelated [label="lpo:PlaceRelated", shape=box];
  PlaceDesc    [label="lpo:PlaceDescription", shape=box];
  When         [label="lpo:When", shape=box];
  Period       [label="lpo:Period", shape=box];
  SourceLabel  [label="lpo:SourceLabel", shape=box];
  Citation     [label="lpo:Citation", shape=box];

  // ===== Datatypen (literals) =====
  string   [label="xsd:string", shape=ellipse, style=dashed];
  anyURI   [label="xsd:anyURI", shape=ellipse, style=dashed];
  gYear    [label="xsd:gYear", shape=ellipse, style=dashed];

  // ===== Place → object properties =====
  Place -> PlaceName    [label="lpo:hasName"];
  Place -> PlaceType    [label="lpo:hasType"];
  Place -> PlaceGeom    [label="lpo:hasGeom"];
  Place -> PlaceRelated [label="lpo:hasRelation"];
  Place -> PlaceDesc    [label="lpo:hasDescription"];
  Place -> When         [label="lpo:hasWhen"];
  Place -> Citation     [label="lpo:hasCitation"];

  // ===== Place → dataproperties =====
  Place -> string [label="lpo:hasTitle"];
  Place -> string [label="lpo:sourceId"];
  Place -> string [label="lpo:sourceDataset"];
  Place -> anyURI [label="lpo:sourceDatasetURI"];
  Place -> string [label="lpo:hasCountryCode"];
  Place -> string [label="lpo:hasFeatureClass"];
  Place -> string [label="lpo:label", style=dotted];

  // ===== PlaceName =====
  PlaceName -> string [label="lpo:toponym"];
  PlaceName -> string [label="lpo:language"];

  // ===== PlaceType & SourceLabel =====
  PlaceType -> anyURI     [label="lpo:identifier"];
  PlaceType -> string     [label="lpo:typeLabel"];
  PlaceType -> SourceLabel[label="lpo:hasSourceLabel"];

  SourceLabel -> string [label="lpo:labelValue"];
  SourceLabel -> string [label="lpo:labelLanguage"];

  // ===== PlaceGeom =====
  PlaceGeom -> string [label="lpo:wktValue"];
  PlaceGeom -> string [label="lpo:s2CellId"];
  PlaceGeom -> string [label="lpo:reprPoint"];
  PlaceGeom -> string [label="lpo:certainty"];

  // ===== PlaceRelated =====
  PlaceRelated -> anyURI [label="lpo:relationType"];
  PlaceRelated -> anyURI [label="lpo:relationTo"];
  PlaceRelated -> string [label="lpo:certainty"];

  // ===== PlaceDescription =====
  PlaceDesc -> string [label="lpo:descriptionValue"];
  PlaceDesc -> string [label="lpo:language"];

  // ===== When =====
  When -> gYear  [label="lpo:sourceYear"];
  When -> string [label="lpo:duration"];
  When -> string [label="lpo:start"];
  When -> string [label="lpo:end"];
  When -> string [label="lpo:earliest"];
  When -> string [label="lpo:latest"];

  // ===== Period =====
  Period -> string [label="lpo:periodName"];
  Period -> anyURI [label="lpo:periodURI"];

  // ===== Citation =====
  Citation -> anyURI [label="lpo:citationURI"];
  Citation -> gYear  [label="lpo:sourceYear"];
}
