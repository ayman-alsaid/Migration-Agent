# Technical Case Study — Migration Agent

## The engineering problem

Migration information is both fragmented and time-sensitive. A programme can change eligibility conditions, move to a different page, close, reopen, or be described inconsistently across official and unofficial sources.

That creates two distinct technical problems:

1. **Trustworthy source maintenance** — what information is current and authoritative?
2. **Meaningful matching** — how does a candidate's actual profile compare with the current requirements?

Treating either problem as a simple LLM prompt produces an attractive demo and an unreliable system.

## Design thesis

Migration Agent was built around a stricter rule:

> **The model may understand information. The architecture must decide when that information is trustworthy enough to act on.**

The system therefore splits the workflow into deterministic and probabilistic responsibilities.

```text
AI responsibilities
- language understanding
- structured extraction
- semantic similarity
- letter generation
- interview simulation

Deterministic / evidence responsibilities
- source authority
- source verification
- content-change detection
- age / language / hard eligibility gates
- refresh state
- evidence links
- submission boundary
```

## Why a verify-first catalogue

The simplest architecture would be:

```text
user asks → LLM searches / recalls → answer
```

That architecture cannot guarantee that an opportunity exists now, that its source is official, or that the requirements have not changed.

Migration Agent instead uses:

```text
candidate programme
    ↓
official-source discovery
    ↓
URL scoring / authority preference
    ↓
scrape + verify
    ↓
structured extraction
    ↓
persist with source evidence
```

The model is useful inside the pipeline but is not itself the evidence layer.

## Change awareness as a first-class feature

A verified catalogue becomes stale the moment it stops being maintained.

Each source is periodically re-crawled and compared to its previous content hash. An unchanged source does not justify another expensive extraction pass. A changed source does.

This is an important architectural distinction:

```text
freshness ≠ scrape everything again
freshness = detect change cheaply, reprocess surgically
```

The source documentation estimates that this Delta-Hash design avoids roughly 95% of unnecessary LLM reprocessing under the project's present assumptions. This is an **ESTIMATED** architectural saving, not a generalized production benchmark.

## Cost-aware extraction

No single scraper is economical across every government website.

Migration Agent uses an escalation model:

```text
Tier 1: Scrapling
  local / lowest cost
        ↓ only on failure
Tier 2: Scrape.do
  proxy-assisted
        ↓ only on failure
Tier 3: Apify
  cloud browser / highest capability and cost
```

The goal is not to maximize scraper sophistication. It is to apply the cheapest sufficient method to each source.

## Matching: why semantic similarity alone is unsafe

Semantic search addresses terminology mismatch:

- `machine learning systems`
- `digital technology professional`

may describe related profiles even without literal keyword overlap.

But semantic similarity is only a **candidate-ranking signal**.

A candidate can rank highly and still be ineligible because of a hard condition. Therefore matching is layered:

```text
candidate profile
    ↓ embedding
vector similarity
    ↓
ranked programmes
    ↓
deterministic gates
    ↓
evidence-backed shortlist
```

Hard gates such as age or language thresholds belong in code because their failure state is not probabilistic.

## Asynchronous operations

Scraping, parsing, embeddings, extraction, and model operations are moved out of normal synchronous web requests into Celery workers backed by Redis.

This isolates:

- latency-heavy work;
- cost-bearing work;
- retryable work;
- scheduled refresh work;

from the API path that serves already-computed data.

## Preparation without autonomous submission

Migration Agent deliberately stops before government-portal submission.

That boundary trades spectacle for safety:

- avoids unnecessary anti-bot/account risk;
- preserves a final human review checkpoint;
- reduces the chance that software silently submits an incorrect claim;
- keeps the system positioned as preparation and decision support rather than autonomous legal action.

This is an example of a deliberate non-feature being stronger engineering evidence than another automation checkbox.

## What I built vs what I used

External components include FastAPI, Celery, Redis, PostgreSQL, pgvector, React, TypeScript, Vite, OpenRouter, Gemini embeddings, Scrapling, Scrape.do, and Apify.

The original engineering contribution is the system around those components:

- verify-first ingestion sequence;
- authority-aware source discovery;
- tiered scraping escalation;
- Delta-Hash change detection;
- structured programme normalization;
- semantic matching plus deterministic gates;
- background refresh and recomputation;
- semi-autonomous safety boundary.

## Current evidence

The project documentation records:

- 131 programmes across 31 countries;
- a live backend and programme catalogue;
- deployed web interface with API integration as an active milestone;
- verify-first ingestion;
- three-tier scraping;
- Delta-Hash refresh;
- pgvector matching;
- deterministic eligibility gates;
- application-letter generation;
- structured 10-question mock interview;
- agency-oriented multi-client support.

What it does **not** yet support as evidence:

- a broad labeled benchmark of matching precision/recall;
- large-scale longitudinal user-outcome data;
- a claim that any broader domain adaptation is already implemented.

## The hidden platform: verify-first, change-aware matching

The strongest generalization in the approved use-case research is not "immigration software can be reused for something else." It is that the architecture solves a recurring information problem:

```text
an official rule exists somewhere
        ↓
it must be found and verified
        ↓
it changes over time
        ↓
it must be normalized
        ↓
a person / company / shipment / project must be matched to it
        ↓
hard conditions must constrain semantic reasoning
```

The research maps this pattern to:

- export documentation and customs compliance;
- cross-border healthcare licensing;
- renewable-energy permitting;
- multi-state franchise registration;
- company incorporation across jurisdictions;
- insurance producer licensing;
- research-grant and funding eligibility.

These examples are useful because they reveal the true abstraction boundary. They are not claims of deployed verticals.

## Lesson

The central lesson from Migration Agent is that high-stakes AI systems become more trustworthy when model capability is surrounded by **explicit authority boundaries**.

The impressive part is not that the AI can answer.

The important part is that the system knows what the AI is **not allowed to decide**.
