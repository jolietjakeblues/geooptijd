# Linked Pasts Ontology (LPO) 1.1 – Uitgebreide structuur en datagebruik

## 🧭 Overzicht

Deze Markdown bevat een tekstuele en hiërarchische representatie van de LPO-ontologie (versie 1.1, Richard Light, 2020).  
Naast de conceptuele structuur bevat dit document nu ook **praktische informatie over gebruik, verplichting, multipliciteit en datamodellering**.

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

## 6. Datagebruik, verplichtingen en cardinaliteit

### 🧩 geojson:FeatureCollection

| Eigenschap | Range | Verplicht | Meervoudig | Beschrijving |
|-------------|--------|------------|--------------|---------------|
| `lpo:hasFeature` | `geojson:Feature` | ✅ ja | ✅ ja | Een verzameling features die samen een collectie vormen. |

---

### 📍 geojson:Feature

| Eigenschap | Range | Verplicht | Meervoudig | Beschrijving |
|-------------|--------|------------|--------------|---------------|
| `geojson-t:geometry` | `lpo:Setting` | ✅ ja | ❌ nee (1 geometry per feature) | De geometrie en bijbehorende ruimte-tijdcontext. |

---

### 🌍 lpo:Setting

| Eigenschap | Range | Verplicht | Meervoudig | Beschrijving |
|-------------|--------|------------|--------------|---------------|
| `lpo:when` | `lpo:Timespan` | ⚙️ optioneel | ❌ nee (functioneel) | Tijd waarin de Setting geldig is. |
| `lpo:setting` | `geojson:Point` | ⚙️ optioneel | ❌ nee | Punt dat de locatie aangeeft. |

> Een `lpo:Setting` kan dus één ruimtelijke geometrie en één tijdsperiode hebben.

---

### ⏳ lpo:Timespan

| Eigenschap | Range | Verplicht | Meervoudig | Beschrijving |
|-------------|--------|------------|--------------|---------------|
| `lpo:has_start` | `time:ProperInterval` | ⚙️ optioneel | ❌ nee | Begin van de periode |
| `lpo:has_end` | `time:ProperInterval` | ⚙️ optioneel | ❌ nee | Einde van de periode |
| `lpo:earliest` | `time:DateTimeDescription` | ⚙️ optioneel | ✅ ja | Vroegste mogelijke datum |
| `lpo:latest` | `time:DateTimeDescription` | ⚙️ optioneel | ✅ ja | Laatste mogelijke datum |
| `lpo:in` | `time:DateTimeDescription` | ⚙️ optioneel | ✅ ja | Moment binnen de tijdsperiode |
| `lpo:period` | `lpo:PeriodDefinition` | ⚙️ optioneel | ✅ ja | Historische periode (bijv. uit PeriodO) |

---

### 📜 Attestation-klassen

| Klasse | Eigenschappen | Verplicht | Meervoudig | Beschrijving |
|---------|----------------|------------|--------------|---------------|
| `lpo:NameAttestation` | `lpo:toponym`, `lpo:has_certainty`, `lpo:source_label`, `lpo:when` | ✅ toponym | ⚙️ rest optioneel | Getuigt van een naam die ergens gebruikt werd. |
| `lpo:TypeAttestation` | `lpo:has_certainty`, `lpo:source_label` | ⚙️ optioneel | ✅ ja | Getuigt van een classificatie of type. |
| `lpo:RelAttestation` | `lpo:relation_type`, `lpo:relation_to`, `lpo:has_certainty`, `lpo:source_label` | ✅ relation_type + relation_to | ✅ ja | Getuigt van een relatie tussen entiteiten (bv. "onderdeel van"). |
| `lpo:LinkAttestation` | `lpo:source_label`, `lpo:has_certainty` | ⚙️ optioneel | ✅ ja | Getuigt van een externe koppeling (bijv. URI). |

---

### 🕰️ Functionele eigenschappen

| Eigenschap | Betekenis |
|-------------|------------|
| `lpo:when` | Functioneel – slechts één tijdsperiode per Setting. |
| `geojson-t:geometry` | Functioneel – één geometrie per Feature. |

---

### 📚 Afleidbare gebruiksregels

| Regel | Betekenis |
|-------|------------|
| Een `Feature` **moet** minimaal één geometrie (`geojson-t:geometry`) hebben. |
| Een `Setting` **mag** maar één tijdsinterval (`lpo:when`) hebben. |
| Een `Timespan` **heeft minimaal één tijdsaanduiding** (`earliest`, `in`, of `period`). |
| Een `NameAttestation` **heeft altijd een toponym**. |
| Certainty en source_label **kunnen herhaald worden** als er meerdere bronnen zijn. |

---

## 7. RDF-voorbeeld

```ttl
:Feature1 a geojson:Feature ;
    geojson-t:geometry [
        a lpo:Setting ;
        lpo:when [
            a lpo:Timespan ;
            lpo:earliest "1795-01-01"^^xsd:date ;
            lpo:latest "1814-01-01"^^xsd:date
        ] ;
        lpo:setting "POINT(4.89 52.37)"^^geo:wktLiteral
    ] ;
    lpo:name_attestation [
        a lpo:NameAttestation ;
        lpo:toponym "Departement van de Schelde" ;
        lpo:source_label "Bron: Franse administratie" ;
        lpo:has_certainty "certain"
    ] .
```

---

## 8. Kernbegrippen in natuurlijke taal

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
