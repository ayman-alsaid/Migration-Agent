# Evidence Index

This directory indexes the public evidence claims used by the Migration Agent case study.

## Implemented / documented

- Verify-first programme catalogue pipeline
- Official-source discovery and URL preference logic
- Three-tier scraper: Scrapling → Scrape.do → Apify
- Delta-Hash change detection
- PostgreSQL + pgvector semantic matching
- Deterministic eligibility gates
- Celery / Redis asynchronous processing
- CV profile extraction
- Application-letter generation
- Structured mock interview flow
- Agency-oriented multi-client mode

## Documented current scope

- 131 immigration programmes
- 31 countries
- Backend and programme catalogue live in project source documentation
- Frontend deployed with live API connection described as an active integration milestone

## Estimated

- Approximately 95% reduction in unnecessary LLM reprocessing from Delta-Hash change detection relative to reprocessing every source on every refresh.

This is **ESTIMATED**, not a generalized benchmark.

## Not yet validated

- Large labeled candidate-programme matching benchmark
- Calibrated acceptance-probability model
- Broad real-world user outcome study
- Production implementations of the non-immigration domain adaptations

## Research-supported adaptation map

The approved AgentCraft use-case analysis explores the architecture in:

- export documentation / customs compliance;
- healthcare licensing;
- renewable-energy permitting;
- franchise registration;
- company incorporation;
- insurance licensing;
- research grants.

These are architecture mappings only. See `docs/DOMAIN_ADAPTATION.md`.

## Evidence discipline

The public repository follows the rule:

**Use the strongest claim the evidence supports — never the strongest claim the product story would benefit from.**
