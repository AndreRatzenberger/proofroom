# Build Sequence

Proofroom should earn its way from a local CLI to integrations.

## Phase 1: Static Casefile

No model calls.

Build:

- diff parser;
- work contract schema;
- evidence item schema;
- Markdown output;
- JSON output;
- manual verdict rules.

This phase proves the product object.

## Phase 2: Repository Context Manifest

Build:

- changed file listing;
- likely affected surface hints;
- protected path detection;
- owner hints from repository files when available;
- suggested tests;
- explicit skipped surfaces.

This phase makes Proofroom more useful than a PR summary.

## Phase 3: Verdict Engine

Build:

- risk tiers;
- required evidence by risk tier;
- auto-accept only for narrow reversible scopes;
- hold for missing evidence;
- escalate for protected surfaces;
- deterministic rule explanations.

This phase turns the casefile into an acceptance decision.

## Phase 4: Pull Request Integration

Build:

- GitHub Action;
- short PR comment;
- full casefile artifact;
- optional status check.

The local CLI proves the product. The PR integration gives it distribution.

## Phase 5: Model Explainer

Build:

- casefile summary;
- recommended next checks;
- reviewer-language explanations;
- optional issue or prompt extraction.

The model is last on purpose. The acceptance decision should already be
grounded in deterministic evidence.

## What Not To Build Yet

Do not start with:

- agent orchestration;
- a dashboard;
- multi-agent staffing;
- memory promotion;
- an agent marketplace;
- a replacement for GitHub review.

Those may become useful later. The first job is the casefile.
