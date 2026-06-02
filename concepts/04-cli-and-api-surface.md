# CLI and API Surface

Proofroom should start boring.

The first version should run locally, inspect a git diff, collect explicit
evidence, and write Markdown plus JSON casefiles.

## CLI Shape

```bash
proofroom casefile create \
  --diff origin/main...HEAD \
  --prompt issue.md \
  --agent codex \
  --out proofroom/casefiles/PR-42.md
```

Add evidence:

```bash
proofroom evidence add PR-42 \
  --command "uv run pytest tests/test_timer_component.py -q" \
  --scope "timer precision behavior"
```

Decide:

```bash
proofroom decide PR-42
```

Show the result:

```bash
proofroom casefile show PR-42
proofroom pr-comment PR-42
```

## Contract Input

The first contract can be YAML:

```yaml
goal: "Fix the timer precision bug"
source: "issue #42"
risk: "medium"
allowed_scope:
  - "src/scheduling/**"
required_evidence:
  - "targeted tests"
  - "diff review"
  - "no frontend changes"
```

The CLI should also support flags for the smallest useful path:

```bash
proofroom casefile create \
  --goal "Fix timer precision bug" \
  --diff origin/main...HEAD \
  --allow "src/scheduling/**" \
  --require "targeted tests" \
  --require "diff review"
```

## Python Shape

```python
from proofroom import Casefile, Evidence, Risk

casefile = (
    Casefile.from_diff("origin/main...HEAD")
    .contract(
        goal="Fix the timer precision bug",
        risk=Risk.medium,
        allowed_scope=["src/scheduling/**"],
    )
    .evidence(
        Evidence.command(
            "uv run pytest tests/test_timer_component.py -q",
            scope="timer precision behavior",
        )
    )
    .decide()
)
```

## Output Files

For a casefile id `PR-42`, Proofroom should be able to write:

```text
proofroom/casefiles/PR-42.md
proofroom/casefiles/PR-42.json
```

The Markdown file is for humans. The JSON file is for automation and future
integrations.

## Pull Request Comment

The PR comment should be short:

```text
Proofroom: hold

Missing:
- scheduling integration test
- owner review for src/scheduling/**

Evidence:
- targeted unit test passed
- diff stayed inside declared scope

Next:
- run scheduling integration suite
```

The full casefile can be linked from the comment when a GitHub integration
exists.
