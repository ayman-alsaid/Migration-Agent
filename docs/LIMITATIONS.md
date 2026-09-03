# Limitations

Migration Agent is intentionally documented with explicit limits because the cost of overstating reliability is high in immigration and compliance-heavy workflows.

## 1. Broad real-user validation is not complete

The system architecture and catalogue are implemented, but a diverse, large-scale user study measuring practical matching usefulness and downstream outcomes has not yet been documented.

## 2. Matching quality is not yet benchmarked against a large labeled corpus

Semantic ranking plus deterministic gates is a strong architectural design, but public evidence does not currently include a large precision/recall benchmark over labeled candidate-programme pairs.

## 3. The ~95% Delta-Hash saving is an estimate

The mechanism is implemented and its logic is clear, but the percentage should be treated as **ESTIMATED** until backed by controlled longitudinal measurements.

## 4. Government-source structure is inconsistent

Official websites vary in HTML quality, bot protection, page organization, document format, and stability. The three-tier scraper reduces this risk but does not eliminate it.

## 5. Scanned and difficult documents remain harder than native text

OCR-heavy or poorly structured documents require additional handling and may be less reliable than normal HTML/text sources.

## 6. Verification does not equal legal advice

Even a current official source can require legal interpretation, exceptions, or fact-specific judgment. The platform helps structure research and preparation; it does not replace qualified advice.

## 7. Fit scores are not approval probabilities

A matching or interview-preparation score must not be interpreted as a guarantee or calibrated prediction of a government decision unless separate outcome validation establishes that relationship.

## 8. Domain adaptation is not plug-and-play

The architecture maps well onto customs, licensing, grants, permitting, and other official-rule domains, but those verticals have not been built. Each requires its own source map, ontology, deterministic rules, experts, and validation.

## 9. Semi-autonomous operation is intentional

The lack of auto-submission is not a missing feature to hide. It is an explicit safety boundary.

## 10. Frontend/backend integration status must remain current

The source documentation describes the backend/catalogue as live and the web interface as deployed with API connection in progress. Public claims should be updated when that integration state changes rather than silently upgrading it.
