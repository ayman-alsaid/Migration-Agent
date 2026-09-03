# Engineering Decisions

## ADR-001 — Official-source verification before publication

**Decision:** A programme cannot enter the trusted catalogue solely because an LLM named it or a search result mentioned it.

**Rejected alternative:** Let the model retrieve and summarize opportunities directly.

**Why:** Fluency and retrieval relevance do not establish authority or freshness.

**Trade-off:** Slower, more complex ingestion with explicit failure states.

---

## ADR-002 — Delta-Hash change detection

**Decision:** Hash normalized source content and reprocess only when the content changes.

**Rejected alternative:** Re-run extraction and embedding against every source on every refresh.

**Why:** Most sources do not change on every crawl; repeated LLM work is wasteful.

**Trade-off:** Additional state, invalidation logic, and maintenance complexity.

**Evidence boundary:** The documented ~95% reduction is an estimate under current assumptions, not a generalized benchmark.

---

## ADR-003 — Cost-aware scraper escalation

**Decision:** `Scrapling → Scrape.do → Apify`, escalating only on failure.

**Rejected alternative:** Use the most capable cloud-browser path for every page.

**Why:** Capability and cost should be proportional to source difficulty.

**Trade-off:** Multiple integrations and more complex error classification.

---

## ADR-004 — Semantic ranking plus deterministic eligibility

**Decision:** Use vector similarity to discover semantically relevant programmes, then apply hard eligibility gates in code.

**Rejected alternative:** Ask the LLM for one holistic eligibility score.

**Why:** Hard rules such as age and language thresholds have unambiguous failure states that should not be probabilistic.

**Trade-off:** Domain rules must be explicitly modeled and maintained as programmes change.

---

## ADR-005 — PostgreSQL + pgvector instead of a separate managed vector database

**Decision:** Keep embeddings beside structured records in PostgreSQL.

**Why:** At the documented scale, the simpler operational topology outweighed introducing another managed dependency.

**Trade-off:** A future scale profile may justify re-evaluating this choice.

---

## ADR-006 — Background workers for expensive operations

**Decision:** Scraping, parsing, embeddings, extraction, and other long-running/cost-bearing operations run asynchronously through Celery/Redis.

**Rejected alternative:** Perform the entire pipeline inside web requests.

**Why:** API responsiveness and operational isolation matter more than implementation simplicity.

**Trade-off:** More job state, queue monitoring, and retry semantics.

---

## ADR-007 — Human submission checkpoint

**Decision:** The platform does not submit immigration applications to government portals automatically.

**Rejected alternative:** End-to-end autonomous application submission.

**Why:** Anti-bot risk, account risk, advisory/legal boundaries, and the importance of a final human review.

**Trade-off:** Less automation and a less “magical” demo.

---

## ADR-008 — Domain-agnostic core, domain-specific authority adapters

**Decision:** Keep discovery, verification, change detection, normalization, matching, and gate orchestration reusable while treating authority definitions and rule schemas as domain-specific.

**Why:** The same engineering problem recurs in customs, licensing, permitting, grants, and similar rule-heavy workflows.

**Trade-off:** Adaptation still requires substantive domain work; portability is architectural, not plug-and-play.
