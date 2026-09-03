# Domain Adaptation — Beyond Immigration

Migration Agent is the first implemented vertical of a broader **verify-first, change-aware matching engine**.

The portable core is not “visa logic.” It is this pattern:

```text
authoritative source
  → verify
  → monitor for change
  → extract structured requirements
  → match a profile/entity
  → apply deterministic gates
  → present evidence-backed result
```

The approved use-case research in the AgentCraft content base tested this pattern against several other domains. The conclusion is intentionally conservative: the architecture transfers; the domain rules do not.

## 1. Export documentation and customs compliance

**Matched entity:** shipment / exporter / product

**Official sources:** customs authorities, trade ministries, chambers, treaty/rules-of-origin documentation.

**Possible gates:** origin rules, destination requirements, certificate type, HS classification dependencies, document validity.

**Why the pattern fits:** the rules are official, jurisdiction-specific, change over time, and mistakes can invalidate paperwork or delay shipments.

**What changes:** source authorities, trade-document schema, tariff/product logic, jurisdiction model, audit trail requirements.

---

## 2. Cross-state / cross-border healthcare licensing

**Matched entity:** clinician / credential profile

**Official sources:** licensing boards, professional compacts, ministries/regulators.

**Possible gates:** profession, current license, compact membership, education, exam, renewal/CE status.

**Why the pattern fits:** a professional profile is matched against jurisdiction-specific requirements that can change independently.

**What changes:** regulator graph, profession-specific rule sets, license-status monitoring, much stricter compliance validation.

---

## 3. Renewable-energy permitting

**Matched entity:** project / site / technology profile

**Official sources:** local, state/provincial, and federal permitting authorities.

**Possible gates:** project size, technology, location, zoning, environmental thresholds, authority jurisdiction.

**Why the pattern fits:** requirements change not only in content but sometimes in **which authority controls the decision**.

**What changes:** hierarchical jurisdiction resolution, geospatial rules, effective dates, permit dependencies.

---

## 4. Multi-state franchise registration

**Matched entity:** franchisor / expansion jurisdiction

**Official sources:** state/provincial registration authorities and statutory sources.

**Possible gates:** registration state, filing type, disclosure version, material-change triggers, renewal status.

**Why the pattern fits:** requirements vary by jurisdiction and can trigger new filing obligations when business conditions change.

---

## 5. Company incorporation across jurisdictions

**Matched entity:** founder/company formation profile

**Official sources:** company registries, gazettes, ministries, district registries.

**Possible gates:** entity type, founders, capital, publication, local representative, filing authority.

**Why the pattern fits:** superficially similar processes hide different authorities and post-incorporation obligations.

---

## 6. Insurance producer licensing

**Matched entity:** producer / agency

**Official sources:** insurance regulators and licensing systems.

**Possible gates:** resident vs nonresident status, client jurisdiction, CE, appointment requirements, renewal windows.

**Why the pattern fits:** compliance must be continuously checked, not only at onboarding.

---

## 7. Research grants and funding eligibility

**Matched entity:** researcher / institution / project profile

**Official sources:** government funders, universities, foundations, official calls.

**Possible gates:** career stage, citizenship/residency, institutional affiliation, degree status, deadline, thematic scope.

**Why the pattern fits:** high-cost applications are often lost because applicants discover a hard eligibility mismatch too late.

## The abstraction boundary

What transfers:

- discovery orchestration;
- authority verification pattern;
- tiered source extraction;
- change detection;
- structured requirement storage;
- embeddings / semantic retrieval;
- deterministic gate framework;
- evidence presentation;
- scheduled refresh and affected-match recomputation.

What does **not** transfer without redesign:

- source-authority rules;
- entity/profile schema;
- extracted requirement ontology;
- deterministic conditions;
- legal and safety boundaries;
- effective-date semantics;
- validation datasets;
- subject-matter review.

## Why this is not “easy repurposing”

The use-case analysis explicitly rejects the idea that a second vertical is a weekend configuration exercise.

A credible adaptation needs:

1. domain experts who can define what “correct” means;
2. authoritative-source mapping;
3. real rule modeling;
4. adversarial edge cases;
5. validation against actual domain examples;
6. governance appropriate to the stakes.

The value of the existing architecture is that the **expensive system-design questions have already been separated cleanly from domain-specific content**. It reduces the scope of a second vertical; it does not eliminate it.

## Portfolio thesis

Migration Agent demonstrates a reusable principle:

> **When official rules change and getting them wrong is expensive, AI should sit behind verification, change awareness, and deterministic controls.**

Immigration proves the vertical. The domain-adaptation research reveals the platform underneath it.
