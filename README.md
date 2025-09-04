# DFO Salmon Ontology  
**Namespace:** https://w3id.org/foc/salmon# (dfo:)  
**License:** CC-BY 4.0  
**Status:** Starter, modular, WebProtégé-ready  

We're developing this modular ontology for DFO salmon data. It builds on the Darwin Core Conceptual Model ecosystem while adding DFO-specific stock assessment concepts and vocabularies. Our goal is to make integration and analysis easier for scientists, while keeping everything interoperable with external communities like GBIF Backbone, ENVO, and OBIS and other  salmon data producing institutions.

---

## Table of Contents  
- Overview  
- Namespace, Licensing, and Versioning  
- Repository Layout  
- Quickstart (WebProtégé)  
- Modeling Principles  
- Ontology Modules  
- IRI Policy  
- W3ID Setup  
- Competency Questions & Example SPARQL  
- Contributing  
- Roadmap (GitHub Issues)  
- Acknowledgments  

---

## Overview  

This ontology is our attempt to create a domain-fit semantic layer for DFO salmon data. Our overall approach is to **start simple and modular, while staying compatible with the larger semantic web ecosystem**.  

We use **Darwin Core (DwC)** and **DwC-DP** as the ecological backbone, since these are widely adopted by biodiversity and ecology communities. From there, we add **DFO-specific layers** for stock assessment, vocabularies, genetics and governance.  

Key technologies and tools we rely on:  
- **Turtle (.ttl)** for ontology modules (human-readable, machine-processable).  
- **SKOS** for controlled vocabularies (method names, categories, terminology).  
- **OWL** for the ontology data model (classes and relationships).  
- **QUDT/OM** for unambiguous units.  
- **W3ID** for persistent, stable IRIs that can be dereferenced.  
- **WebProtégé** for collaborative editing and community review.  
- **WebVOWL** for visualization and teaching/explaining model structure.  
- **GitHub** for version control, issue tracking, and structured contributions.  

**Design philosophy:**  
- Start with the **minimal viable structure** to represent stock assessment data in a machine-readable way.  
- Use **community review** (via word docs, SKOS vocabularies and WebProtégé editing) to refine and expand.  
- Maintain **modularity**: Core, Stock Assessment, and vocabularies are in separate `.ttl` files, but they are combined into a single merged ontology (`dfo-salmon.ttl`) for easier viewing/editing in WebProtégé or WebVOWL.  
- Ensure that edits in WebProtégé/WebVOWL are not treated as the sole “source of truth.” Any approved changes must also be updated in the **module-specific `.ttl` files** in this repository, not the merged `dfo-salmon.ttl` file.  

This dual system—**easy merged viewing/editing** plus **modular maintenance**—gives flexibility while keeping the ontology organized and sustainable.  

---

## Namespace, Licensing, and Versioning  
- **Base IRI / Namespace:** https://w3id.org/foc/salmon# (prefix dfo:)  
- **Owner/Maintainer:** DFO Pacific Science — Data Stewardship Unit  
- **License:** CC-BY 4.0  

**Versioning**  
- I include `owl:versionInfo` in the ontology header.  
- For releases, we will add an `owl:versionIRI` (e.g., https://w3id.org/foc/salmon/1.0).  
- We will tag releases in GitHub (v1.0.0) and may archive via Zenodo to assign a DOI.  

---

## Repository Layout  
/dfo-salmon-ontology
├── README.md ← This file \
├── dfo-salmon.ttl ← Top-level ontology; imports modules \
├── dfo-salmon-core.ttl ← Core classes/properties (shared) \
├── dfo-salmon-stock.ttl ← Stock assessment module \
├── vocab-escapement-methods.ttl ← SKOS scheme from DFO spreadsheet \
├── /docs/ ← HTML docs (pyLODE/WIDOCO) \
├── /mapping/ ← term mapping sheets, sample data \
└── /w3id/ ← .htaccess for w3id


---

## Modeling Principles  

Ontologies are only useful if they:  
1. Are **consistent** (no contradictory logic).  
2. Are **interoperable** (play well with external vocabularies and ontologies).  
3. Use **persistent identifiers (IRIs)** that can be referenced across systems and time.  
4. Support both **human understanding** (via labels, definitions, and vocabularies) and **machine reasoning** (via OWL axioms).  

### IRIs — What and Why  
- An **IRI** (Internationalized Resource Identifier) is the globally unique identifier for a concept, class, or individual.  
- Example: `https://w3id.org/foc/salmon#Stock/SkeenaSockeye`  
- IRI's derefernce the identifier (IRI) from the location of the file (URL) to maintain a constant id to point at and redirect as necessary
- Without IRIs, we’d only have local names. Publishing IRIs allow datasets and models to **link up**, which is the entire point of the semantic web.  

### Our Specific Choices  
- **DwC vs dwciri:** We use `dwc:` terms for literals (strings, numbers, dates) and `dwciri:` terms for links to IRIs (e.g., taxonomic references in GBIF). This keeps us aligned with Darwin Core conventions while supporting linked data.  
- **Units:** We require QUDT/OM IRIs (e.g., `unit:Each`) for count units. Strings like `"fish"` are ambiguous and hurt interoperability.  
- **SKOS + OWL hybrid:** SKOS gives us maintainable vocabularies; OWL gives us structural logic. By combining them, we can let communities review and maintain vocabularies without needing to edit complex OWL.  
- **Modularity:** Each domain area (Core, Stock Assessment, Vocabularies, Genetics, Governance) is in its own file, but they can be merged for editing and visualization.  

---

## Ontology Modules  
- **Core:** foundational classes and properties (`dfo:Stock`, `dfo:ConservationUnit`, `dfo:Dataset`, `dfo:Indicator`).  
- **Stock Assessment:** subclasses for measurement types (e.g., `dfo:WeirCountMeasurement`) and survey events.  
- **Vocabularies:** SKOS ConceptSchemes (e.g., escapement methods) for consistent terminology across datasets.  
- **Future: Genetics:** representation of GSI outputs, samples, markers, protocols.  
- **Future: Governance / Provenance:** using PROV-O to track dataset creation, stewardship roles, and provenance.  

**Editing workflow:** We merge all modules into one file for WebProtégé/WebVOWL viewing and editing. But once edits are agreed on, they must be applied back into the module-specific `.ttl` files, which remain the source of truth.  

---

## IRI Policy  
- **Base:** https://w3id.org/foc/salmon#  
- **Instances:** use HTTP IRIs for Stocks, Events, Measurements, Datasets.  
- **Taxa:** link to GBIF Backbone IRIs.  
- **Locations/Agents:** literals for now, IRIs later as needed.  

---

## W3ID Setup  
We configure `/w3id/foc/salmon/` with `.htaccess` redirects for Turtle, HTML, and fallbacks. Then we submit a PR to [w3id.org](https://github.com/perma-id/w3id.org).  

---

## Competency Questions & Example SPARQL  

When designing this ontology, I defined **competency questions** — the queries we want the ontology to be able to answer. These questions shaped the design of classes, properties, and vocabularies.  

We grouped them into themes:  

### 1. **Monitoring Coverage and Effort**  
- *How many populations (stocks) were monitored in a given year?*  
- *Which survey events occurred in a given year and region?*  

These drove the creation of `dfo:SurveyEvent`, `dfo:EscapementMeasurement`, and the property `dfo:eventDate`.  

### 2. **Trends and Abundance**  
- *What is the trend in regional abundance of Sockeye?*  
- *Can escapement datasets from multiple river systems be aggregated consistently?*  

This motivated `dfo:countValue`, `dfo:countUnit`, and controlled vocabularies for methods. By standardizing units and methods, we can aggregate across systems.  

### 3. **Data Products and Indicators**  
- *Which datasets contributed to FSAR indicators updated in the last 12 months?*  
- *Which datasets are linked to specific indicators or benchmarks?*  

This required `dfo:Dataset` (aligned with schema:Dataset) and `dfo:Indicator`, with properties linking datasets to indicators and metadata.  

### 4. **Genetics and Provenance** (future modules)  
- *Which reports cite datasets derived from GSI collections (2019–2021)?*  
- *Who created or updated a dataset, and how was it processed?*  

This is why we plan to include PROV-O patterns (`prov:Activity`, `prov:Agent`) and genetics-specific classes.  

**Overall design decision:** We only included questions that require interoperability across datasets, governance tracking, or long-term findability. These are the areas where an ontology adds the most value compared to a simple relational database.  

---

## Contributing  
- Keep core, modules, and vocabularies in separate TTLs.  
- Use `dfo:` for local terms, `dwc:` for literals, and `dwciri:` for IRI links.  
- Prefer external IRIs when available; only mint DFO-specific terms when necessary.  
- Pull requests should include reasoning and examples.  
- Turtle style: one triple per line; English labels and comments.  

**Code of Conduct:** Be constructive and evidence-based.  

---

## Roadmap (GitHub Issues)  
We track tasks here with labels like `module`, `vocabulary`, `infra`, `docs`, `question`. Upcoming items include:  
- Infra: w3id redirects  
- Docs: publish HTML docs  
- Modules: genetics, governance/provenance  
- Vocabulary: expand escapement methods  
- Alignment: GBIF Backbone integration  
- QA: SHACL shapes and GitHub Action validation  
- Catalogue: schema.org crosswalks  
- Policy: add benchmarks and reference points  
- **TODO:** Update all Turtle files to replace `https://w3id.org/dfo/` with `https://w3id.org/foc/`.  
