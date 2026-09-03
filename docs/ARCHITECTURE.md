# Architecture

## Topology

```text
                    ┌───────────────────────────────┐
                    │ React + TypeScript + Vite UI │
                    └──────────────┬────────────────┘
                                   │ REST / HTTPS
                                   ▼
                    ┌───────────────────────────────┐
                    │            FastAPI            │
                    │ auth · profiles · matching    │
                    │ letters · interview · plans   │
                    └──────────────┬────────────────┘
                                   │
                     enqueue / read computed state
                                   │
                    ┌──────────────▼────────────────┐
                    │       Redis + Celery          │
                    │ workers + scheduled refresh   │
                    └───────┬────────┬──────────────┘
                            │        │
                 ┌──────────▼─┐   ┌──▼───────────────┐
                 │ Scraping    │   │ AI / Embeddings  │
                 │ 3-tier      │   │ extraction etc.  │
                 └──────┬──────┘   └──────┬───────────┘
                        │                 │
                        └─────────┬───────┘
                                  ▼
                    ┌───────────────────────────────┐
                    │ PostgreSQL 15 + pgvector      │
                    │ programmes · sources · users  │
                    │ embeddings · change state     │
                    └───────────────────────────────┘
```

## Catalogue ingestion flow

```text
candidate programme
  → source discovery
  → candidate URL scoring
  → official-source preference
  → three-tier scrape
  → verification
  → normalization
  → content hash
  → structured extraction
  → embedding
  → persisted programme + source evidence
```

A failed source is not silently converted into a valid programme record. Failure is a state that must remain visible to the maintenance workflow.

## Refresh flow

```text
scheduled crawl
    ↓
fetch source
    ↓
normalize content
    ↓
compute hash
    ├── unchanged → stop; no extraction / embedding refresh
    └── changed   → re-extract → update programme → recompute affected matches
```

The architecture treats source maintenance as an ongoing system responsibility, not a data-import task completed once.

## Candidate matching flow

```text
CV PDF
  → structured profile
  → profile embedding
  → cosine similarity against programme embeddings
  → candidate ranking
  → deterministic eligibility gates
  → fit / acceptance score
  → evidence-backed programme results
```

The vector layer expands recall across vocabulary differences. The deterministic layer prevents semantic similarity from being mistaken for eligibility.

## Mock interview flow

```text
candidate + target context
  → interview state in Redis
  → strict-officer prompt
  → adaptive 10-question exchange
  → transcript
  → structured evaluation
       acceptance probability
       strengths
       weaknesses
       critical mistakes
       model answers
```

The interview is a preparation tool, not a predictor of an actual government decision.

## Component responsibilities

### FastAPI
Owns user-facing API boundaries, authorization, read paths, task submission, and access to already-computed state.

### Celery workers
Own operations that are slow, expensive, retryable, or scheduled: scraping, extraction, parsing, embeddings, refresh, and similar workflows.

### Redis
Supports worker queues and low-latency transient state such as interview sessions.

### PostgreSQL + pgvector
Keeps structured programme/source data and vector representations in one persistence system.

### Scraper stack
Implements cost-aware escalation rather than one universal scraping strategy.

### LLM / embedding providers
Handle probabilistic understanding tasks. They are not the authority layer.

## Trust boundaries

### Authority boundary
A model response does not prove that a programme exists. Authority is earned through verified source evidence.

### Eligibility boundary
Semantic relevance does not override hard programme conditions.

### Freshness boundary
A previously verified record cannot be assumed current indefinitely; refresh state is part of validity.

### Automation boundary
The platform prepares and explains. It does not autonomously submit government applications.

## Generalized architecture

Removing immigration vocabulary yields:

```text
authoritative source discovery
  → verification
  → extraction
  → change monitoring
  → structured rule store
  → profile/entity matching
  → deterministic gates
  → evidence-backed result
```

That is the reusable platform boundary described in the domain-adaptation research.
