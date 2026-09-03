# Security and Safety Boundaries

Migration Agent operates in a high-stakes information domain. Its safety posture is therefore defined as much by what it refuses to automate as by what it can do.

## Authority separation

The LLM does not establish that a programme exists or that a source is official.

Source authority is resolved through the verification pipeline. This prevents model fluency from being treated as evidence.

## Eligibility separation

Semantic matching expands recall, but hard requirements remain deterministic.

This prevents a probabilistic similarity score from overriding a bright-line condition such as age or language threshold.

## Freshness separation

Previously verified information is not assumed valid forever. Scheduled refresh and content-change detection are part of the system's trust model.

## Human submission checkpoint

The system does not autonomously submit applications to government portals.

This protects against:

- anti-bot/account sanctions;
- silent submission of incorrect data;
- loss of user review before a consequential action;
- blurred legal/advisory boundaries.

## Cost-bearing operations

Expensive scraping and AI operations are isolated through background workers and cost-aware escalation. This reduces the risk that a single user-facing request can unintentionally trigger unbounded processing.

## Evidence presentation

Recommendations should remain linked to verifiable source material so users can independently inspect the basis of the result.

## High-stakes disclaimer

Migration Agent is a research, matching, and preparation system. It is not a government decision-maker, does not provide a guarantee of acceptance, and should not replace qualified legal or immigration advice where that is needed.

## Broader-domain safety rule

The domain-adaptation architecture cannot be transferred responsibly by changing labels alone.

Each new vertical requires:

- domain experts;
- authority mapping;
- effective-date semantics;
- domain-specific deterministic gates;
- validation against real edge cases;
- appropriate legal and operational governance.

**Reusable architecture does not mean reusable authority.**
