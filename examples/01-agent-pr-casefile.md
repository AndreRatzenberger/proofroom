# Example Casefile: Timer Precision Bug

```yaml
id: PR-42
status: draft
verdict: hold
```

## Work Contract

```yaml
goal: "Fix the timer precision bug"
source: "issue #42"
risk: "medium"
allowed_scope:
  - "src/scheduling/**"
required_evidence:
  - "targeted timer tests"
  - "diff review"
  - "no frontend changes"
```

## Agent

```yaml
agent:
  name: "codex"
  runtime: "coding-agent"
  claimed_task_complete: true
```

## Change Manifest

```yaml
diff_range: "origin/main...HEAD"
files_changed:
  - "src/scheduling/timer.py"
  - "tests/test_timer_component.py"
outside_declared_scope: []
protected_paths_touched: []
```

## Evidence

```yaml
evidence:
  - type: "command"
    command: "uv run pytest tests/test_timer_component.py -q"
    status: "passed"
    scope: "timer precision behavior"
    does_not_cover:
      - "full daily schedule integration"
      - "frontend rendering"
  - type: "diff_review"
    status: "present"
    scope: "changed scheduling files"
```

## Missing Evidence

```yaml
missing_evidence:
  - "scheduling integration suite"
```

## Risk Classification

```yaml
risk:
  tier: "medium"
  reasons:
    - "changes runtime scheduling behavior"
    - "targeted test exists"
    - "integration coverage missing"
```

## Verdict

```yaml
verdict:
  status: "hold"
  reasons:
    - "required integration evidence missing"
  next_checks:
    - "run scheduling integration suite"
```

## Reviewer Summary

The patch is inside declared scope and has targeted test evidence, but it
changes scheduling behavior without integration evidence. The work may be good.
It is not yet strong enough to count automatically.
