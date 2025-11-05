# Linked Pasts Ontology (LPO) 1.1 – Tekstuele structuur

## 🧭 Overzicht

Deze Markdown bevat een tekstuele en hiërarchische representatie van de LPO-ontologie (versie 1.1, Richard Light, 2020).  
Het doel is om de structuur, relaties en semantische samenhang binnen LPO inzichtelijk te maken.

---

## 1. Topniveau: GeoJSON-structuur

```ttl
geojson:FeatureCollection
 └── lpo:hasFeature → geojson:Feature
       "Verzameling van Features binnen een FeatureCollection"
```

```ttl
geojson:GeometryCollection
 └── lpo:setting → geojson:Point
       "De setting (locatiepunt) van een GeometryCollection"
```

---

## 2. Kernklassen

### lpo:Setting
> Beschrijft de ruimtelijke en/of temporele scope van een entiteit  
> (zoals een plaats, historische periode of gebeurtenis).

Relaties:
```
- Kan een geojson:Point bevatten (via lpo:setting)
- Wordt vaak gebruikt als 'geometry' binnen geojson-t:geometry
```

---

### lpo:Timespan
> Subklasse van `time:ProperInterval`.

Eigenschappen:
```
- lpo:has_start → time:ProperInterval
- lpo:has_end → time:ProperInterval
- lpo:earliest → time:DateTimeDescription
- lpo:latest → time:DateTimeDescription
- lpo:in → time:DateTimeDescription   (te verduidelijken)
- lpo:period → lpo:PeriodDefinition
```

Toepassing:
```
- Wordt gebruikt door lpo:when en lpo:timespan om geldigheidsduur of bestaan uit te drukken.
```

---

### lpo:PeriodDefinition
> Definieert een historische periode (bijv. uit PeriodO).

```
rdfs:isDefinedBy → https://test.perio.do/d.json
```

---

### Attestatieklassen

#### lpo:NameAttestation, TypeAttestation, RelAttestation, LinkAttestation
> Allemaal subklassen van `lawd:Attestation`.

Gebruik:
```
- lpo:name_attestation → lpo:NameAttestation
- lpo:type_attestation → lpo:TypeAttestation
- lpo:rel_attestation → lpo:RelAttestation
- lpo:link_attestation → lpo:LinkAttestation
```

Specifieke eigenschappen:
```
lpo:NameAttestation
 └── lpo:toponym → xsd:string   (de eigenlijke naam)
```

---

## 3. Tijd-gerelateerde eigenschappen

```
lpo:when → lpo:Timespan
    "Relateert een entiteit of eigenschap met de periode waarin zij bestond of geldig was."

lpo:timespan → lpo:Timespan
    "Algemene koppeling aan een tijdsinterval."
```

---

## 4. Attestation-eigenschappen

```
lpo:name_attestation → lpo:NameAttestation
lpo:type_attestation → lpo:TypeAttestation
lpo:rel_attestation  → lpo:RelAttestation
lpo:link_attestation → lpo:LinkAttestation
```

Elke Attestation (dus alle vier) kan:
```
- lpo:source_label → xsd:string
- lpo:has_certainty → xsd:string ("certain" | "uncertain")
```

Relatie-attestaties hebben meestal:
```
- lpo:relation_type → URI (bijv. Getty Vocab)
- lpo:relation_to → URI (bijv. gazetteer-URL)
```

---

## 5. GeoJSON-uitbreiding

```
geojson-t:geometry
 ├── subPropertyOf → geojson:geometry
 ├── domain → geojson:Feature
 └── range → lpo:Setting
     "Maakt het mogelijk om bij een Feature niet alleen geometrie maar ook 'when'-informatie op te nemen."
```

---

## 6. Overzicht in samenvattende boom

```
geojson:FeatureCollection
 └── hasFeature → geojson:Feature
      └── geojson-t:geometry → lpo:Setting
           ├── when → lpo:Timespan
           │    ├── has_start → time:ProperInterval
           │    ├── has_end → time:ProperInterval
           │    ├── earliest/latest/in → time:DateTimeDescription
           │    └── period → lpo:PeriodDefinition
           └── setting → geojson:Point
```

```
lawd:Attestation
 ├── NameAttestation (→ toponym)
 ├── TypeAttestation
 ├── RelAttestation (→ relation_type, relation_to)
 └── LinkAttestation
```

---

## 7. Kernbegrippen in natuurlijke taal

| Concept | Beschrijving |
|----------|---------------|
| **Setting** | Contextuele “ruimte-tijd” van iets (plaats, gebeurtenis, naam) |
| **Timespan** | Tijdinterval waarin iets geldig is |
| **Attestation** | Getuigenis of bewijs (bronvermelding voor naam, type, relatie of link) |
| **PeriodDefinition** | Gedefinieerde historische periode (bijv. PeriodO entry) |
| **geojson-t:geometry** | Uitbreiding van GeoJSON waarmee tijd aan geometrie wordt gekoppeld |

---

## 📘 Licentie & Herkomst
Gebaseerd op *Linked Pasts Ontology (LPO) v1.1*, Richard Light, gestart 13 maart 2020.  
Bron: http://linkedpasts.org/ontology#
