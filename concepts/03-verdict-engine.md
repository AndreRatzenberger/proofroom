# Verdict Engine

The verdict engine decides whether an agent-authored change is allowed to count.

It should be deterministic first. Models can explain the result later, but hard
gates should not depend on model confidence.

## Verdicts

### `auto_accept`

The change is narrow, reversible, inside declared scope, and backed by required
evidence.

Example:

```text
Change: typo fix in docs
Risk: low
Evidence: markdown lint passed
Scope: docs only
Verdict: auto_accept
```

### `hold`

The work may be fine, but the casefile is incomplete.

Example:

```text
Change: timer precision bugfix
Risk: medium
Evidence: targeted unit test passed
Missing: scheduling integration test
Verdict: hold
Next: run scheduling integration suite
```

### `escalate`

The change touches protected surfaces, violates scope, changes authority
boundaries, or needs a human owner.

Example:

```text
Change: session-token refresh behavior
Risk: high
Evidence: unit tests passed
Touched: auth middleware
Missing: security review, replay test, rollback note
Verdict: escalate
Owner: security reviewer
```

## Deterministic Inputs

The first engine should inspect:

- changed files;
- allowed scope;
- protected path rules;
- required evidence;
- command status;
- CI status;
- owner review requirements;
- rollback requirements;
- reversibility hints;
- risk tier.

## Basic Rule Order

1. If required evidence failed, return `hold` or `escalate`.
2. If required evidence is missing, return `hold`.
3. If protected paths were touched, return `escalate`.
4. If files changed outside declared scope, return `escalate`.
5. If the change is high-risk, require owner review.
6. If the change is low-risk, narrow, reversible, and fully evidenced, return
   `auto_accept`.
7. Otherwise return `hold`.

## Protected Surface Examples

Protected surfaces should be configurable, but common examples include:

- authentication and authorization;
- payments;
- credentials and secrets;
- migrations;
- production deploy configuration;
- security policy;
- regulated or customer data;
- public contract language;
- destructive data operations.

## Why No `reject` In V0

Most agent work will not be clearly wrong. It will be under-evidenced.

`hold` is the useful default because it turns uncertainty into concrete next
checks instead of pretending the system can judge everything from a summary.
