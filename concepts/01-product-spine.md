# Proofroom Product Spine

## What Proofroom Believes

Proofroom believes the hard part of agentic software work is shifting.

When coding agents are weak, the bottleneck is generation. When coding agents
get stronger, the bottleneck becomes acceptance:

```text
Should this generated work enter the system?
```

That question needs more than a diff summary. It needs a structured case.

## Product Object

The `Casefile` is the product.

It joins:

- the work contract;
- the changed artifact;
- the context manifest;
- the authority path;
- the evidence bundle;
- the risk classification;
- missing evidence;
- the verdict;
- recommended next checks.

The casefile turns scattered traces into a decision surface.

## What It Refuses To Become

Proofroom refuses to become:

- a prompt library;
- another chat surface;
- a generic AI code reviewer;
- a transcript viewer;
- a project management wrapper;
- a compliance dashboard that produces paperwork but no decisions.

The product only owns acceptance.

## Core Questions

A Proofroom casefile should answer:

- What was the requested work?
- What changed?
- Was the change inside declared scope?
- Which context did the agent appear to use?
- What evidence exists?
- What evidence is missing?
- Did the change touch protected surfaces?
- What risk tier is this?
- What should happen next?

## Design Principles

### Evidence Before Belief

Work is not accepted because a model says it is done. Work is accepted when the
required evidence exists.

### Deterministic First

Protected paths, required checks, failed commands, missing evidence, and scope
violations should be boring rule outputs before any model explains them.

### Missing Evidence Is Evidence

If no test command is recorded, the casefile should say so. If CI did not run,
that absence matters. If the agent changed files outside scope, that is not a
stylistic concern. It changes the verdict.

### Short Comment, Full Record

The pull request comment should be short enough for a busy reviewer to read.
The full casefile should be available when something feels off.

## Success Standard

Proofroom is working when a reviewer can answer in under one minute:

```text
Can this agent work count?
If not, what exact evidence is missing?
```
