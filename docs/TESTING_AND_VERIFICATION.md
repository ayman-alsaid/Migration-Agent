# Testing and Verification

Migration Agent is a verification-heavy system, so this document separates what the current evidence supports from what still requires broader validation.

## Verified / implemented behavior

### Source verification pipeline

Programme records are tied to source discovery, URL scoring, scraping, verification, and structured extraction before being trusted.

### Explicit scrape failure handling

Failed source acquisition is recorded as failure rather than silently converted into valid catalogue data.

### Change detection

Source content is normalized and hashed. A changed hash triggers reprocessing; an unchanged hash skips unnecessary extraction.

### Semantic matching + hard rules

Profile/programme similarity is handled through vector matching while hard eligibility conditions are enforced separately in deterministic logic.

### Asynchronous heavy work

Long-running or cost-bearing operations are isolated behind background workers rather than blocking the normal API request path.

## Documented current operational evidence

The project source documents:

- **131 programmes across 31 countries** in the maintained catalogue;
- live backend and programme catalogue;
- deployed frontend with live API connection as an integration milestone;
- three-tier scraping;
- pgvector-based semantic matching;
- application-letter workflow;
- structured mock interview workflow;
- agency-oriented multi-client mode.

These are project-scope / implementation claims, not third-party product benchmarks.

## Estimated evidence

### Delta-Hash LLM savings

The source documentation describes approximately **95% reduction in unnecessary LLM reprocessing** compared with blindly re-extracting every page on every refresh.

Classification: **ESTIMATED**.

Why it is not labeled MEASURED here: the repository evidence available for this public case study does not include a controlled longitudinal cost study or independently audited production telemetry establishing a generalized percentage.

## Not yet validated

### Matching benchmark

A large labeled dataset measuring precision, recall, false positives, and false negatives across candidate-programme pairs has not yet been documented.

### Outcome prediction

The platform must not present its fit or interview scores as guarantees of real immigration approval outcomes.

### Diverse real-user study

A broad validation study with a diverse candidate cohort remains on the roadmap.

### Domain adaptations

Customs, licensing, permitting, grants, insurance, and related adaptations are researched architecture mappings. They are **not implemented verticals**.

## What evidence would strengthen the next phase

Future evidence should ideally include:

- source-refresh logs across a defined period;
- changed vs unchanged page ratios;
- actual LLM cost before/after Delta-Hash over the same source set;
- labeled matching evaluation;
- gate-failure audit examples;
- stale-source detection cases;
- diverse user testing;
- agency workflow testing;
- false-positive / false-negative analysis.

## Review rule

A successful demo is not enough to prove a high-stakes matching system. The evidence standard is:

**Claim → Status → Evidence → Scope → Limitation**
