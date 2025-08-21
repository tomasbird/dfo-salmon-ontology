
# DFO Salmon Ontology

**Namespace:** `https://w3id.org/dfo/salmon#` (`dfo:`)  
**License:** CC-BY 4.0  
**Status:** Starter, modular, WebProtégé-ready

This repository hosts a modular ontology for DFO salmon data that reuses the Darwin Core / Darwin Core Data Package (DwC/DwC-DP) ecosystem while adding DFO-specific stock assessment concepts, vocabularies, and hooks for genetics and governance. The design goal is simple: **make integration and analysis easier for scientists, while remaining interoperable with external communities (GBIF Backbone, ENVO, OBIS, etc.).**

---

## Table of Contents
- [Overview](#overview)
- [Namespace, Licensing, and Versioning](#namespace-licensing-and-versioning)
- [Repository Layout](#repository-layout)
- [Quickstart (WebProtégé)](#quickstart-webprotégé)
- [Modeling Principles](#modeling-principles)
  - [DwC vs `dwciri:` (literals vs IRIs)](#dwc-vs-dwciri-literals-vs-iris)
  - [Units (QUDT/OM)](#units-qudtom)
  - [Modularization](#modularization)
  - [SKOS + OWL “hybrid”](#skos--owl-hybrid)
- [Ontology Modules](#ontology-modules)
  - [Core](#core)
  - [Stock Assessment](#stock-assessment)
  - [Vocabularies](#vocabularies)
  - [Future: Genetics](#future-genetics)
  - [Future: Governance / Provenance](#future-governance--provenance)
- [IRI Policy](#iri-policy)
- [W3ID Setup](#w3id-setup)
- [Competency Questions & Example SPARQL](#competency-questions--example-sparql)
- [Contributing](#contributing)
- [Roadmap (GitHub Issues)](#roadmap-github-issues)
- [Acknowledgments](#acknowledgments)

---

## Overview

This ontology provides a **domain-fit semantic layer** for DFO salmon data, centered on:
- **Integration & interoperability** (align stocks, methods, and variables across datasets; use GBIF Backbone for taxa).
- **Operational workflow & governance** (enable stewardship roles and traceable provenance).
- **Discovery & retrieval** (classify datasets, methods, variables; expose metadata for catalog search).

The ontology **reuses DwC/DwC-DP** as the ecological backbone (classes like Event, Occurrence; relationships mirrored as local DFO properties) and adds DFO-specific layers for **stock assessment**, **vocabulary alignment**, and **future genetics/governance**.

---

## Namespace, Licensing, and Versioning

- **Base IRI / Namespace:** `https://w3id.org/dfo/salmon#` (prefix `dfo:`)
- **Owner/Maintainer:** DFO Pacific Science — Data Stewardship Unit
- **License:** [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)

### Versioning
- The ontology header includes `owl:versionInfo`. For releases, also include an `owl:versionIRI` (e.g., `https://w3id.org/dfo/salmon/1.0`).
- Tag releases in GitHub (e.g., `v1.0.0`). Optionally archive via Zenodo to assign a DOI.

---

## Repository Layout

```
/dfo-salmon-ontology
├── README.md                ← This file
├── dfo-salmon.ttl           ← Top-level ontology; imports modules
├── dfo-salmon-core.ttl      ← Core classes/properties (shared)
├── dfo-salmon-stock.ttl     ← Stock assessment module (events, counts, indicators)
├── vocab-escapement-methods.ttl  ← SKOS scheme from DFO spreadsheet
├── /docs/                   ← (optional) HTML docs (pyLODE/WIDOCO)
├── /mapping/                ← (optional) term mapping sheets, sample data
└── /w3id/                   ← .htaccess for w3id (see below)
```

> The **top-level ontology** (`dfo-salmon.ttl`) imports the **Core** and **Stock** modules. Genetics and governance modules can be added later in the same style and imported there.

---

## Quickstart (WebProtégé)

1. Open your WebProtégé project (or create a new one).  
2. **Upload** `dfo-salmon.ttl`. (It imports `dfo-salmon-core.ttl` and `dfo-salmon-stock.ttl` automatically.)  
3. To standardize methods across datasets, **upload** `vocab-escapement-methods.ttl` (SKOS) or keep it in the same triple store.  
4. Start adding **your DFO-specific subclasses** and properties in WebProtégé:
   - `dfo:EscapementSurveyEvent ⊑ dfo:SurveyEvent`
   - `dfo:WeirCountMeasurement ⊑ dfo:EscapementMeasurement`
   - `dfo:SonarCountMeasurement ⊑ dfo:EscapementMeasurement`
5. Keep **locations and agents as literals** for now (your stated preference). When ready, switch to `dwciri:` properties for IRIs without changing classes.

---

## Modeling Principles

### DwC vs `dwciri:` (literals vs IRIs)
- Use `dwc:` terms for **literals** (strings, numbers, dates).  
- Use `dwciri:` terms for **IRI links** (e.g., `dwciri:taxonID` → GBIF Backbone IRIs).  
- Avoid using `*ID` literal terms as object properties.

The starter ontology includes `dfo:taxon ⊑ dwciri:taxonID` so a `dfo:Stock` (or related instance) can link to a GBIF Backbone **taxon IRI** cleanly.

### Units (QUDT/OM)
- Counts use `dfo:countValue` (xsd:integer) and `dfo:countUnit` (IRI).  
- Use `unit:Each` (or other QUDT/OM unit IRIs) to avoid ambiguous string units.

### Modularization
- **Core**: shared classes/properties (Stock, CU, MU, SurveyEvent, EscapementMeasurement, Dataset, Indicator).  
- **Stock**: stock-assessment-specific subclasses and comments.  
- **Vocabularies**: SKOS concept schemes for controlled lists (e.g., escapement methods).  
- **Future**: Genetics and Governance/Provenance as separate modules to keep concerns isolated.

### SKOS + OWL “hybrid”
- Use SKOS for **maintainable vocabularies** (method names, categories).  
- Use OWL for the **data model** (classes/relations).  
- The Core provides a few **annotation properties** (e.g., `dfo:expectedRole`, `dfo:granularityLevel`) to record intent when evolving SKOS lists toward OWL patterns without punning.

---

## Ontology Modules

### Core
Core classes and properties shared across modules:
- `dfo:Stock`, `dfo:ConservationUnit`, `dfo:ManagementUnit`
- `dfo:SurveyEvent`, `dfo:EscapementMeasurement`
- `dfo:Dataset` (subClassOf `schema:Dataset`) for catalog alignment
- `dfo:Indicator`
- Key properties:  
  - `dfo:hasConservationUnit`, `dfo:hasManagementUnit`  
  - `dfo:aboutStock` (EscapementMeasurement → Stock)  
  - `dfo:observedDuring` (EscapementMeasurement → SurveyEvent)  
  - `dfo:usesMethod` (EscapementMeasurement → SKOS Concept)  
  - `dfo:eventDate` (SurveyEvent), `dfo:countValue`, `dfo:countUnit`
  - `dfo:region` (literal; temporary for aggregation)

### Stock Assessment
- Subclasses for measurement types: `dfo:WeirCountMeasurement`, `dfo:SonarCountMeasurement`, `dfo:AerialCountMeasurement`.
- Subclass for events: `dfo:EscapementSurveyEvent`.
- Intended to be extended with benchmarks/targets and indicators used by FSAR.

### Vocabularies
- `vocab-escapement-methods.ttl` provides a **SKOS ConceptScheme** derived from DFO spreadsheet content: top concepts → categories → terms.  
- Use these IRIs as the object of `dfo:usesMethod` for **consistent cross-dataset alignment**.

### Future: Genetics
- Classes to model GSI outputs, reporting units (repunits), materials/samples, markers, and protocols.  
- Map reporting units to Conservation Units (WSP) using `skos:exactMatch` / `skos:closeMatch` where appropriate.

### Future: Governance / Provenance
- Adopt **PROV-O** to capture who created/updated a dataset and how it was processed (`prov:Activity`, `prov:Agent`, `prov:wasGeneratedBy`, `prov:used`).  
- Align dataset metadata to **schema.org** for findability; optionally include GeoNetwork-required fields.

---

## IRI Policy

- **Base:** `https://w3id.org/dfo/salmon#` (`dfo:`).  
- **Instances:** Use **HTTP IRIs** for Stocks, Events, Measurements, Datasets. Example patterns (customize in code):  
  - `https://w3id.org/dfo/salmon#Stock/{CU-or-code}`  
  - `https://w3id.org/dfo/salmon#Event/{YYYY}/{Region}/{ID}`  
  - `https://w3id.org/dfo/salmon#Meas/{EventID}/{Type}`
- **Taxa:** Use `dfo:taxon` to link to **GBIF Backbone** IRIs (`dwciri:taxonID` semantics).  
- **Locations/Agents:** Keep as literals initially; introduce IRIs later.

---

## W3ID Setup

1. Create `/w3id/dfo/salmon/` in this repo (or a sibling “infra” repo) with an `.htaccess` such as:
   ```apache
   RewriteEngine On
   # Turtle when requested
   RewriteCond %{HTTP_ACCEPT} text/turtle
   RewriteRule ^$ https://raw.githubusercontent.com/dfo-pacific-science/dfo-salmon-ontology/main/dfo-salmon.ttl [R=302,L]

   # HTML docs if you publish them under /docs/
   RewriteCond %{HTTP_ACCEPT} text/html
   RewriteRule ^$ https://dfo-pacific-science.github.io/dfo-salmon-ontology/ [R=302,L]

   # Fallback
   RewriteRule ^$ https://raw.githubusercontent.com/dfo-pacific-science/dfo-salmon-ontology/main/dfo-salmon.ttl [R=302,L]
   ```
2. Submit a PR to [`https://github.com/perma-id/w3id.org`](https://github.com/perma-id/w3id.org) adding your `/dfo/salmon/` directory.  
3. After merge, **the namespace is live** and will redirect to your TTL/HTML.

---

## Competency Questions & Example SPARQL

> Replace example IRIs/fields with your actual instance IRIs once loaded. Prefixes assumed:
> ```sparql
> PREFIX dfo:   <https://w3id.org/dfo/salmon#>
> PREFIX skos:  <http://www.w3.org/2004/02/skos/core#>
> PREFIX xsd:   <http://www.w3.org/2001/XMLSchema#>
> PREFIX dct:   <http://purl.org/dc/terms/>
> PREFIX prov:  <http://www.w3.org/ns/prov#>
> PREFIX unit:  <http://qudt.org/vocab/unit/>
> ```

### Q1. *How many populations (stocks) were monitored in 2022?*
```sparql
SELECT (COUNT(DISTINCT ?stock) AS ?nStocks)
WHERE {
  ?meas a dfo:EscapementMeasurement ;
        dfo:aboutStock ?stock ;
        dfo:observedDuring ?event .
  ?event dfo:eventDate ?date .
  FILTER (STRSTARTS(STR(?date), "2022"))
}
```

### Q2. *Trend in regional abundance of Sockeye*
```sparql
SELECT ?region (YEAR(?date) AS ?year) (SUM(?count) AS ?regionalTotal)
WHERE {
  ?meas a dfo:EscapementMeasurement ;
        dfo:aboutStock ?stock ;
        dfo:observedDuring ?event ;
        dfo:countValue ?count .
  ?event dfo:eventDate ?date ;
         dfo:region ?region .
  ?stock dfo:taxon ?sockeyeIri .  # GBIF Backbone taxon IRI for Sockeye
}
GROUP BY ?region (YEAR(?date))
ORDER BY ?region ?year
```

### Q3. *Datasets contributing to FSAR indicators updated in last 12 months*
```sparql
SELECT ?dataset ?title ?modified
WHERE {
  ?dataset a dfo:Dataset ;
           dct:title ?title ;
           dct:modified ?modified .
  FILTER (?modified >= (NOW() - "P12M"^^xsd:duration))
  # Link to Indicator if used in your data graph:
  # ?indicator a dfo:Indicator ; dfo:isComputedFrom ?dataset .
}
ORDER BY DESC(?modified)
```

### Q4. *Which reports cite datasets derived from GSI collections (2019–2021)?*
```sparql
SELECT ?report ?dataset
WHERE {
  ?dataset a dfo:Dataset ;
           dct:issued ?issued .
  FILTER("2019-01-01"^^xsd:date <= ?issued && ?issued < "2022-01-01"^^xsd:date)
  # Add your provenance pattern once genetics module is in place:
  # ?dataset prov:wasGeneratedBy ?activity .
  # ?activity dct:subject "GSI" .  # or a more specific property
  ?report dct:references ?dataset .
}
```

### Q5. *Can escapement datasets from multiple river systems be aggregated consistently?*
> Ensure all count records use `dfo:countUnit` with QUDT/OM IRIs (e.g., `unit:Each`) and normalize methods via `dfo:usesMethod` → SKOS.

---

## Contributing

1. **File layout**: keep core, modules, and vocabularies in separate TTLs.  
2. **Prefixes**: use `dfo:` for local classes/properties; reuse `dwc:` for literals and `dwciri:` for IRI links when you add them.  
3. **Vocabularies**: prefer external IRIs (GBIF Backbone, ENVO, etc.); create SKOS concepts only when genuinely DFO-specific.  
4. **Pull requests**: include a brief description, examples, and reasoning.  
5. **Coding style**: Turtle, one triple per line; labels and comments in English (`@en`).

### Code of Conduct
Be constructive and evidence-based. Cite tickets, datasets, or docs when proposing changes.

---

## Roadmap (GitHub Issues)

Create issues in this repo for the following tasks (use labels: `module`, `vocabulary`, `infra`, `docs`, `question`, `help wanted`).

1. **Infra — Set up w3id redirects**  
   - Add `/w3id/dfo/salmon/.htaccess` and open PR to `w3id.org`.  
   - ✅ Accept-header routing for Turtle/HTML.

2. **Docs — Publish HTML**  
   - Generate HTML with pyLODE or WIDOCO into `/docs/`.  
   - Link from README and w3id.

3. **Module — Genetics scaffold**  
   - Classes for GSI: ReportingUnit, Material/Sample, Protocol, Assay, Sequence.  
   - Map ReportingUnits → ConservationUnits via `skos:exactMatch/closeMatch`.

4. **Module — Governance/Provenance scaffold**  
   - Add PROV-O patterns (`prov:Activity`, `prov:Agent`).  
   - Roles: Data Steward, Data Trustee, Product Owner.

5. **Vocabulary — Expand escapement methods**  
   - Validate terms with field teams; add missing synonyms as `skos:altLabel`.  
   - Add `skos:scopeNote` where ambiguity exists.

6. **Alignment — GBIF Backbone integration**  
   - Populate `dfo:taxon` with GBIF IRIs for key stocks/populations.  
   - Consider `owl:sameAs` to WoRMS/ITIS where needed.

7. **Examples — Sample data graph**  
   - Small TTL with 1 Stock, 1 Event (2022), 2 Measurements (weir/sonar), 1 Dataset.  
   - Verify the SPARQL examples run.

8. **QA — SHACL shapes**  
   - Draft SHACL for required fields (`aboutStock`, `observedDuring`, `countUnit`).  
   - Add a GitHub Action to validate PRs.

9. **Catalogue — Schema.org mapping**  
  - Ensure `dfo:Dataset` instances carry `dct:title`, `dct:modified`, keywords.  
  - Crosswalk to GeoNetwork metadata fields.

10. **Policy — Benchmarks & decisions**  
   - Add `Benchmark`, `ReferencePoint`, and links to Indicators.  
   - Support queries like “Can Skeena sockeye support a commercial fishery?”

> Open one issue per task and link them to a high-level **Project** board if desired.

---

## Acknowledgments

- Darwin Core and Darwin Core Data Package communities.  
- GBIF, OBIS, ENVO, QUDT/OM maintainers.  
- DFO Pacific Science — Data Stewardship Unit, for requirements and test cases.

---

## Contact

DFO Pacific Science — Data Stewardship Unit  
Maintainer: Brett Johnson (Program Head, Data Stewardship Unit)  
License: CC-BY 4.0
