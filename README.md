# INSAFEDARE Ontology

## Overview

**OntologySD** is an OWL 2 ontology for representing concepts related to the quality, safety, privacy, and regulatory compliance of synthetic health data. It was developed to support the assessment and governance of synthetic data pipelines — particularly in healthcare contexts where data utility must be balanced against privacy risk, hazard management, and regulatory obligations.

The ontology is draws on a broad set of international standards, including GDPR, the EU AI Act, the European Health Data Space (EHDS) Regulation, HIPAA, ISO/IEC 20889, ISO/IEC 27559, IEC 62304, and NIST frameworks, among others.

---

## Files

| File | Description |
|---|---|
| `ontologySD.ttl` | The ontology in Turtle (RDF) format. Contains all class definitions, object/data properties, individuals, annotations, and SWRL inference rules. |
| `ontologySD.properties` | A Java-style properties file for JDBC database connection configuration (used when loading the ontology from a relational database backend). Fields are intentionally left blank and must be populated for local deployment. |

---

## Namespace

```
http://www.semanticweb.org/ek279783/ontologies/2025/8/ontologySD/
```

Prefix alias: `:` (default in the TTL file)

---

## Key Thematic Areas

The ontology covers six major thematic areas, each modelled as a hierarchy of classes and properties:

### 1. Synthetic Data Types & Generation Methods
Classes representing the full landscape of synthetic data generation approaches, including statistical methods (parametric/non-parametric), rule-based methods, simulation-based methods, machine learning models (GANs, VAEs, diffusion models, transformer-based models), and federated learning variants. Data modalities covered include tabular, time-series, image, text, audio, video, graph, and multimodal data.

### 2. Evaluation Metrics
A rich taxonomy of metrics for assessing synthetic data quality across dimensions such as fidelity, utility, privacy, realism, diversity, fairness, robustness, interpretability, and replicability. Metrics are linked to specific data modalities where applicable.

### 3. Data Hazards & Risk Assessment
A Failure Mode and Effects Analysis (FMEA)-inspired model of data hazards covering data entry/collection failures, storage/management issues, transmission errors, processing and analysis errors, security and privacy breaches, and environmental/contextual factors. Each hazard individual carries severity, likelihood, and detectability scores, and a SWRL rule automatically computes a Risk Priority Number (RPN) and classifies hazards as low, moderate, or high risk.

### 4. Privacy & De-identification
Classes and properties for privacy models (k-anonymity, l-diversity, t-closeness, differential privacy), de-identification techniques (suppression, generalisation, randomisation, pseudonymisation, cryptographic tools), and re-identification risk metrics. Benchmark thresholds drawn from ISO/IEC 27559 Annex B are instantiated for public/non-public, individual/group data scenarios.

### 5. Compliance & Assurance
An assurance case structure (Claim → Context → Evidence → Justification) covering data quality, data lifecycle management, governance, security and privacy, AI model robustness, explainability, fairness, and adversarial attack resilience. Each assurance claim is linked to relevant regulatory standards and produces timestamped evidence logs.

### 6. Semantic Privacy Guarantees
High-level semantic guarantees (non-identifiability, contextual anonymity for recipients, data minimisation by design, purpose-bound data usability, enforceability of access constraints, re-evaluability over time, verified resistance to re-identification attacks, and synthetic data equivalence for permitted use) modelled as named individuals of `SemanticPrivacyGuarantees`, each supported by evidence and regulated by specific legislative articles.

---

## SWRL Rules

Three inference rules are included:

| Rule | Description |
|---|---|
| `RPNCalculation` | Computes `HasRPN` = `HasSeverityScore` × `HasLikelihoodScore` × `HasDetectabilityScore` for any `DataHazards` individual. |
| `LowRisk` | Classifies a hazard as `LowRiskHazard` when RPN < 150. |
| `ModerateRiskClassification` | Classifies a hazard as `ModerateRiskHazard` when 150 ≤ RPN < 300. |
| `HighRiskClassification` | Classifies a hazard as `HighRiskHazard` when RPN ≥ 300. |

---

## Standards & Regulatory Coverage

The ontology instantiates individuals for over 60 regulatory standards and frameworks, including:

- **EU/International**: GDPR (multiple articles), EHDS Regulation, EU AI Act, EU Ethics Guidelines for Trustworthy AI, OECD AI Principles
- **ISO/IEC**: ISO/IEC 20889, 27559, 27001, 27002, 27005, 25010, 25012, 29100, 29151, 62304, 62366-1, 14971, 13485, 22301, 31000
- **NIST**: SP 800-53, Cybersecurity Framework 2.0, AI RMF, NIST IR 8216, AES-256
- **Healthcare-specific**: HIPAA, DICOM, HL7 FHIR, IHE profiles, IEC 81001-5-1, IMDRF
- **Bias/Fairness**: IEEE 7003-2024, IBM AI Fairness 360, Aequitas, Fairlearn
- **ENISA reports**: Threat Landscape for Healthcare, Smart Hospitals, AI Cybersecurity Challenges

---

## `ontologySD.properties` – Configuration

This file configures JDBC connectivity for environments where the ontology is backed by a relational database. All fields are empty and must be set before use:

```properties
jdbc.url=        # e.g. jdbc:postgresql://localhost:5432/ontologydb
jdbc.user=       # database username
jdbc.password=   # database password
jdbc.driver=     # e.g. org.postgresql.Driver
```

> **Note:** Do not commit credentials to version control. Use environment variables or a secrets manager in production.

---

## Usage

The ontology can be opened and reasoned over with standard OWL tools:

- **Protégé** (desktop) – load `ontologySD.ttl` directly; use a SWRL-capable reasoner (e.g. HermiT or Pellet) to fire the RPN rules.
- **Apache Jena / OWL API** – load the Turtle file programmatically for SPARQL queries or reasoning pipelines.
- **RDFLib (Python)** – parse with `rdflib.Graph().parse("ontologySD.ttl", format="turtle")` for lightweight querying.

---

## Provenance

- **Created**: August 2025  
- **Last updated annotation**: 2020-12-10 (embedded in `lastUpdated` annotation property; reflects referenced NIST SP 800-53 catalogue date)  
- **Base URI**: `http://www.semanticweb.org/ek279783/ontologies/2025/8/ontologySD`  
- **OWL API version**: 4.5.29 (2024-05-13)
