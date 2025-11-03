# 🌍 Termennetwerk Geo-Temporeel Model  
### Een eenvoudige uitleg

## Waarom dit model?

Plaatsen veranderen. Grenzen schuiven, gemeenten fuseren, landen verdwijnen of ontstaan.  
Toch blijven we over die plaatsen praten — in archieven, erfgoeddatabanken, kaarten, of verhalen.

Het Termennetwerk Geo-Temporeel Model helpt ons om **plaats en tijd samen te begrijpen**:
> *Wat was waar, en wanneer was dat zo?*

---

## Wat het doet

Het model maakt het mogelijk om te zeggen:
- **“Weesp (gemeente, 1812–1966)”**  
- **“Limburg (departement Overmaas, ca. 1795–1814)”**  
- **“Veerle (gemeente, 1812, deel van Departement Twee Neten)”**

In plaats van één vaste “Weesp” krijgen we zo meerdere versies door de tijd heen.  
Dat noemen we *places in time* — plaatsen zoals ze op een bepaald moment bestonden.

---

## Waarom is dit belangrijk?

| Organisatie | Kijkt naar plaats vanuit… |
|--------------|---------------------------|
| **RCE / musea** | erfgoedobjecten en hun herkomst |
| **IISG** | historische bestuurlijke structuren |
| **Geonovum** | geografische data en standaarden |
| **NDE / Termennetwerk** | betekenis en termen |

Het geo-temporeel model zorgt dat al die perspectieven samenkomen.  
Iedereen gebruikt dezelfde bouwstenen om plaats + tijd te beschrijven.  
Daardoor kunnen datasets van verschillende instellingen elkaar beter **vinden, vergelijken en combineren**.

---

## Hoe werkt het?

### Drie niveaus

1. **Plaats (Place)**  
   De naam of identiteit door de tijd heen.  
   → “Weesp”

2. **Plaats in de tijd (Place in Time)**  
   De specifieke verschijningsvorm in een periode.  
   → “Weesp (gemeente, 1812–1966)”

3. **Geometrie**  
   De kaart of grens die daarbij hoort.  
   → polygon van de gemeente rond 1950.

Bij elk van die lagen slaan we ook op:
- **wanneer** iets gold (`vanaf – tot`);
- **waar** het deel van was (`deel van provincie Noord-Holland`);
- **waar de informatie vandaan komt** (`Topotijdreis`, `CBS`, `Kadaster`).

---

## Wat levert dat op?

- **Betere koppelingen** tussen erfgoedcollecties, historische kaarten en geografische datasets.  
- **Tijdslagen** die je kunt doorzoeken of visualiseren (“toon Weesp in 1950”).  
- **Eenduidige termen** die zowel betekenis, tijd als plaats in zich dragen.  
- **Transparantie** over de bron en herkomst van informatie (provenance).

---

## Voorbeeld

> 🗓️ **Weesp (gemeente, 1812–1966)**  
> - Bestuurlijk type: gemeente  
> - Geldig van: 1812–01–01 tot 1966–08–01  
> - Deel van: Noord-Holland (provincie, 1840–heden)  
> - Bronnen: Topotijdreis, CBS Gemeentelijke indeling, Kadaster  
> - Herkomst: samengesteld door RCE / Termennetwerk

---

## Technische achtergrond (kort)

- gebaseerd op **SKOS** (voor termen en concepten);  
- uitbreidbaar met **Linked Places Format (LPF)** voor tijd- en ruimtemodellering;  
- compatibel met **GeoSPARQL** en **CIDOC CRMgeo**;  
- gebruikt standaard eigenschappen voor **bron** en **provenance** (Dublin Core, PROV-O).  

---

## Wat dit mogelijk maakt

> Termennetwerk zorgt voor betekenis.  
> Dit model voegt plaats en tijd toe.  
> Samen maken ze erfgoeddata écht contextueel en doorzoekbaar.

---

### Samenwerking

Dit model is een **brug** tussen erfgoed en geodata.  
Het sluit aan bij het werk van:
- **Geonovum** – standaarden voor geodata  
- **IISG** – historische geografie  
- **Netwerk Digitaal Erfgoed / NDE** – semantische interoperabiliteit  
- **RCE** – erfgoedcollecties en context

Iedereen kan eraan bijdragen.  
Samen bouwen we aan een gemeenschappelijke taal voor *plaats door de tijd heen*.
