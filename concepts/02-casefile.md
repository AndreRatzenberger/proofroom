# Casefile Object

A `Casefile` is the structured record for one agent-authored change.

It is not a transcript. It is not a bug report. It is not a vibes summary. It is
the acceptance record.

## Minimal Shape

```yaml
id: PR-42
goal: "Fix the timer precision bug"
source: "issue #42"
agent:
  name: "codex"
  runtime: "coding-agent"
risk: "medium"
verdict: "hold"
```

## Work Contract

The work contract captures what the agent was asked to make true.

```yaml
contract:
  goal: "Fix the timer precision bug"
  source: "issue #42"
  allowed_scope:
    - "src/scheduling/**"
  required_evidence:
    - "targeted timer tests"
    - "diff review"
    - "no frontend changes"
```

The contract can be written manually, derived from an issue or prompt, and then
edited by a human.

## Change Manifest

The change manifest describes what actually changed.

```yaml
change:
  diff_range: "origin/main...HEAD"
  files_changed:
    - "src/scheduling/timer.py"
    - "tests/test_timer_component.py"
  outside_declared_scope: []
  protected_paths_touched: []
```

Proofroom should start with simple git inspection. It does not need deep program
understanding to catch obvious scope and evidence problems.

## Context Manifest

The context manifest records what the casefile could inspect.

```yaml
context:
  repo_instructions:
    - "AGENTS.md"
    - "README.md"
  likely_affected_surfaces:
    - "daily scheduling"
    - "timer rollover"
  skipped_surfaces:
    - "full integration suite not run"
```

This is where Proofroom differs from a short PR summary. It names the edges of
what is known.

## Evidence Bundle

Every evidence item should carry scope.

```yaml
evidence:
  - type: "command"
    command: "uv run pytest tests/test_timer_component.py -q"
    status: "passed"
    scope: "timer precision behavior"
    does_not_cover:
      - "full daily schedule integration"
      - "frontend rendering"
```

Evidence can include:

- commands;
- CI status;
- tests;
- static analysis;
- screenshots;
- traces;
- build artifacts;
- reviewer notes;
- owner approvals.

## Missing Evidence

Missing evidence is explicit:

```yaml
missing_evidence:
  - "scheduling integration suite"
  - "owner review for src/scheduling/**"
```

Proofroom should not hide gaps behind optimistic prose.

## Verdict

The verdict is the acceptance decision:

```yaml
verdict:
  status: "hold"
  reasons:
    - "required integration evidence missing"
  next_checks:
    - "run scheduling integration suite"
```

The verdict should be explainable by deterministic facts first.
