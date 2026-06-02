# Proofroom

Proofroom is an acceptance layer for agent-authored work.

Agents can write code, draft changes, and open pull requests. Proofroom decides
whether that work is allowed to count.

It turns a generated change into a reviewable casefile that says what was
requested, what changed, what evidence exists, what risk remains, and whether
the result should be accepted, held, or escalated.

## One-Sentence Thesis

As coding agents get better, code generation stops being the scarce resource.
Acceptance becomes scarce.

## The Problem

An agent opens a pull request that looks plausible.

The reviewer now has to reconstruct the run from:

- the original prompt or issue;
- the diff;
- chat history;
- terminal output;
- tests that may or may not have run;
- missing context the agent did not notice;
- a confident summary written by the same actor that produced the change.

That reconstruction is the bottleneck.

Proofroom exists so reviewers can answer quickly:

```text
Can this agent-authored work count?
If not, what exact evidence is missing?
```

## What Proofroom Produces

Proofroom v0 emits:

- a Markdown casefile;
- a machine-readable JSON casefile;
- a short pull request comment;
- a verdict: `auto_accept`, `hold`, or `escalate`;
- missing evidence;
- recommended next checks.

The casefile is not a transcript dump. It is a decision record for one piece of
agent-authored work.

## Verdicts

```text
auto_accept
```

The change is narrow, reversible, inside declared scope, and backed by required
evidence.

```text
hold
```

The change may be good, but the casefile is incomplete.

```text
escalate
```

The change touches protected surfaces, violates scope, changes authority
boundaries, or needs a human owner.

## Minimal CLI Sketch

```bash
proofroom casefile create \
  --diff origin/main...HEAD \
  --prompt issue.md \
  --agent codex \
  --out proofroom/casefiles/PR-42.md

proofroom evidence add PR-42 \
  --command "uv run pytest tests/test_timer_component.py -q" \
  --scope "timer precision behavior"

proofroom decide PR-42
```

## Minimal Python Sketch

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

print(casefile.verdict)
```

## What Proofroom Is Not

Proofroom is not:

- an AI code reviewer;
- a coding agent;
- a chat UI;
- a project management system;
- a general governance dashboard;
- a replacement for human engineering judgment.

AI code review asks whether a diff contains bugs. Proofroom asks whether the
case is strong enough for the work to count.

## Current Status

Concept seed. No implementation yet.

The first useful build is a local CLI that can create static casefiles from a
git diff, a work contract, and evidence items. The model comes later. The first
version should work without any model call.

## Start Here

- [Product spine](concepts/01-product-spine.md)
- [Casefile object](concepts/02-casefile.md)
- [Verdict engine](concepts/03-verdict-engine.md)
- [CLI and API surface](concepts/04-cli-and-api-surface.md)
- [Build sequence](concepts/05-build-sequence.md)
- [Example casefile](examples/01-agent-pr-casefile.md)
- [Example PR comment](examples/02-pr-comment.md)
