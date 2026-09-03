# Migration Agent — Verify-First Opportunity Matching

![Verify First](https://img.shields.io/badge/Verify_First-official_sources-5B6F8A?style=flat-square)
![Change Aware](https://img.shields.io/badge/Change_Aware-delta_hash-5F7A72?style=flat-square)
![Deterministic Gates](https://img.shields.io/badge/Deterministic_Gates-eligibility-6B6F86?style=flat-square)
![Engineering Evidence](https://img.shields.io/badge/Public_Repository-engineering_evidence-4F6B7A?style=flat-square)

> **What if an AI system were useful precisely because it refused to guess?**

Migration Agent is a working full-stack system for matching a candidate's professional profile against immigration opportunities backed by official government sources.

The important part is not that an LLM can read a CV or write an application letter. The engineering thesis is stricter:

**AI may understand, extract, translate, rank, and explain. It does not get to decide whether an opportunity exists, whether a source is authoritative, or whether a hard eligibility condition has been satisfied.**

That authority belongs to a surrounding verification architecture.

```text
Official source
    ↓
Discover → Verify → Scrape → Normalize → Detect Change
    ↓
Structured requirements
    ↓
Semantic matching
    ↓
Deterministic eligibility gates
    ↓
Evidence-backed recommendation
```

## Why this matters

In immigration, stale or unsupported information is not a cosmetic defect. A single changed age limit, language threshold, points rule, salary threshold, deadline, or programme status can make weeks of preparation worthless.

Migration Agent was built around two failure modes:

1. **Stale / unverifiable information** — an article or intermediary page can remain online long after the official rule changed.
2. **Shallow matching** — keyword search can miss semantically relevant programmes while surfacing programmes that fail hard eligibility conditions.

The system therefore combines **verified-source ingestion**, **change detection**, **semantic matching**, and **deterministic gates** instead of treating an LLM answer as the product.

## Current documented scope

The source documentation records a catalogue of **131 immigration programmes across 31 countries**. Candidate CVs are parsed into structured profiles, matched semantically against the catalogue, then filtered through hard eligibility logic. The system also supports application-letter generation, a structured mock visa interview, and an agency-oriented multi-client mode.

The backend and programme catalogue are documented as live. The web interface is deployed and its connection to the live API is an active integration milestone. Broad real-world validation across a diverse user cohort is **not yet complete**.

## The six design decisions that define the project

### 1. Verify before recommending

A model-generated programme name is not accepted as evidence. The catalogue pipeline independently discovers likely official URLs, scores source candidates, prefers government domains, scrapes the selected source, verifies it, extracts structured requirements, and only then stores the programme.

**Fluency is not authority.**

### 2. Change-aware data instead of a static catalogue

Government pages are rechecked on a schedule. A content hash is compared with the previous version. Unchanged pages skip expensive re-extraction; changed pages re-enter the extraction and matching pipeline.

This makes the catalogue a maintained system rather than a one-time dataset.

The project's source material describes an **approximately 95% reduction in unnecessary LLM reprocessing** from this Delta-Hash strategy. That figure is treated here as **ESTIMATED**, not as an independently audited benchmark.

### 3. Cheap-first scraping escalation

Government sites vary from simple static pages to aggressively protected sites.

```text
Scrapling (local / cheapest)
      ↓ on failure
Scrape.do (proxy)
      ↓ on failure
Apify (cloud browser / most expensive)
```

The system spends more only when the source actually requires it.

### 4. Semantic similarity is not eligibility

Embeddings solve vocabulary mismatch. They do **not** prove eligibility.

A candidate may semantically resemble a programme and still fail an age, language, or other hard condition. These bright-line rules are evaluated deterministically in code.

```text
semantic relevance ≠ legal / programme eligibility
```

### 5. Local vector search

Programme embeddings and candidate embeddings live beside the structured records in PostgreSQL through `pgvector`, avoiding a separate managed vector database for the current scale and architecture.

### 6. Semi-autonomous by design

Migration Agent prepares; it does not submit applications to government portals on the user's behalf.

This is a deliberate non-feature. Automatic submission introduces anti-bot risk, account risk, legal / advisory boundary issues, and removes a human checkpoint from a high-stakes process.

## System architecture

```text
React + TypeScript + Vite
          │ HTTPS / REST
          ▼
       FastAPI
  ┌───────┼────────┬─────────────┐
  │       │        │             │
Profiles Matching Letters   Interview
  │       │        │             │
  └───────┴────┬───┴─────────────┘
               ▼
         Redis / Celery
               │
   ┌───────────┼──────────────┐
   ▼           ▼              ▼
Scraping   Extraction     Embeddings
   │                           │
   └──────────────┬────────────┘
                  ▼
       PostgreSQL + pgvector
                  │
                  ▼
     official-source catalogue
```

Heavy work — scraping, parsing, embeddings, extraction, and other cost-bearing operations — is pushed into background workers so the user-facing API is not forced to hold long-running work open.

## Core product capabilities

| Capability | Role in the system |
|---|---|
| CV ingestion | Turns a PDF résumé into a structured professional profile |
| Official-source catalogue | Maintains programme records tied to verifiable sources |
| Semantic matching | Handles vocabulary mismatch between profile and programme language |
| Deterministic gates | Enforces hard requirements outside probabilistic reasoning |
| Live fit / acceptance scoring | Recomputes when relevant programme criteria change |
| Application letters | Generates destination-language support material plus user translation |
| Mock interview | Runs a structured 10-question preparation interview with a report |
| Agency mode | Supports multiple managed clients under one operating account |

## The broader engine hiding underneath immigration

Immigration is the **first implemented vertical**. The deeper asset is a reusable architecture for domains where:

- authoritative rules live in external official sources;
- those rules change over time;
- decisions depend on matching a specific profile/entity to those rules;
- unsupported answers are expensive;
- semantic understanding helps, but hard rules must still remain deterministic.

The reusable core is:

```text
Discover official sources
        ↓
Verify authority
        ↓
Extract structured requirements
        ↓
Detect change over time
        ↓
Match entity ↔ requirements
        ↓
Apply deterministic gates
        ↓
Present evidence
```

The approved use-case research maps this pattern onto **export documentation & customs compliance, cross-border professional licensing, renewable-energy permitting, franchise registration, company incorporation across jurisdictions, insurance producer licensing, and research-grant eligibility**.

These are **adaptation opportunities, not shipped products**. Each requires its own authority model, source discovery logic, domain experts, rule schema, deterministic gates, and validation programme.

See [`docs/DOMAIN_ADAPTATION.md`](docs/DOMAIN_ADAPTATION.md) for the detailed mapping.

## What transfers — and what does not

What can transfer architecturally:

- official-source discovery;
- source verification;
- tiered extraction;
- hash-based change detection;
- structured normalization;
- vector matching;
- deterministic rule gates;
- evidence presentation;
- asynchronous refresh.

What **must** change per domain:

- what counts as authoritative;
- who or what is being matched;
- the rules being extracted;
- the deterministic gate logic;
- temporal / jurisdiction semantics;
- safety, legal, and escalation policies;
- validation datasets and subject-matter review.

**Reusable architecture does not mean interchangeable domains.**

## Evidence status

| Claim | Status |
|---|---|
| Full-stack Migration Agent architecture | **IMPLEMENTED** |
| Backend and programme catalogue | **DEPLOYED** in source documentation |
| 131 programmes / 31 countries | **DOCUMENTED CURRENT SCOPE** |
| Verify-first catalogue pipeline | **IMPLEMENTED** |
| Three-tier scraper | **IMPLEMENTED** |
| Delta-Hash change detection | **IMPLEMENTED** |
| ~95% unnecessary LLM reprocessing reduction | **ESTIMATED** |
| pgvector semantic matching | **IMPLEMENTED** |
| Deterministic eligibility gates | **IMPLEMENTED** |
| Broad labeled matching-quality benchmark | **NOT YET VALIDATED** |
| Diverse real-user outcome study | **NOT YET VALIDATED** |
| Customs / licensing / grants verticals | **RESEARCHED ADAPTATIONS — NOT IMPLEMENTED** |

## Engineering evidence

- [`docs/CASE_STUDY.md`](docs/CASE_STUDY.md) — problem, constraints, decisions, trade-offs, contribution
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — topology, flows, responsibilities, trust boundaries
- [`docs/ENGINEERING_DECISIONS.md`](docs/ENGINEERING_DECISIONS.md) — ADR-style decisions and rejected alternatives
- [`docs/DOMAIN_ADAPTATION.md`](docs/DOMAIN_ADAPTATION.md) — the verify-first engine beyond immigration
- [`docs/TESTING_AND_VERIFICATION.md`](docs/TESTING_AND_VERIFICATION.md) — what is verified and what remains unvalidated
- [`docs/SECURITY_AND_SAFETY.md`](docs/SECURITY_AND_SAFETY.md) — authority boundaries, automation limits, safety posture
- [`docs/LIMITATIONS.md`](docs/LIMITATIONS.md) — known constraints and technical debt
- [`evidence/README.md`](evidence/README.md) — evidence index and claim boundaries

## Live system

**Migration Agent:** https://migration.agentcraft.info

## Source code & review scope

The production source code for this project is maintained privately and is not distributed through this public repository.

This repository is maintained as an **Engineering Evidence / Technical Case Study**. It exists so technical reviewers can evaluate architecture, decisions, verification boundaries, operational design, evidence status, and limitations without exposing proprietary implementation details or credentials.

## Disclaimer

Migration Agent provides informational guidance and preparation support. It is **not legal advice**, does not guarantee immigration outcomes, and does not replace official government sources or qualified professional advice. Users should independently verify current requirements before acting.

---

**Ayman Alsaid · AgentCraft**  
https://agentcraft.info · contact@agentcraft.info
