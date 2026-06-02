# SFD Decision Log

## Surface Type

CLI and developer API for an agent-work acceptance layer.

## Convergence Status

Iterating. Initial public concept surface created on 2026-06-02.

## Decisions

- 2026-06-02: Start Proofroom as a standalone public repository. Reason: the
  acceptance casefile is a sharper public wedge than a broader agent-work
  institution.
- 2026-06-02: Keep the first version deterministic and local. Reason: the core
  product is the casefile and verdict, not model explanation.
- 2026-06-02: Use `Casefile` as the central product object. Reason: it joins the
  contract, diff, context, evidence, missing evidence, risk, and verdict.
- 2026-06-02: Use `auto_accept`, `hold`, and `escalate` as the first verdicts.
  Reason: most generated work is not clearly wrong; it is under-evidenced.
- 2026-06-02: Keep the pull request comment short and the full casefile separate.
  Reason: busy reviewers need a fast decision surface, with details available
  when needed.

## Derived Contracts

Provisional only:

- `Casefile`: acceptance record for one agent-authored change.
- `WorkContract`: goal, source, allowed scope, risk, required evidence.
- `ChangeManifest`: diff range, changed files, protected paths, scope status.
- `EvidenceItem`: command, CI result, review, screenshot, trace, or artifact
  with explicit scope.
- `Verdict`: `auto_accept`, `hold`, or `escalate` plus reasons and next checks.

## Hardening Status

- [ ] Persistence (currently: concept only)
- [ ] CLI implementation (currently: sketch)
- [ ] Domain logic (currently: verdict rules described)
- [ ] Error handling (currently: not implemented)
- [ ] GitHub integration (currently: planned)
