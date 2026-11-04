---
name: ooda-loop
description: >-
  OODA loop for website performance, scalability, cost, errors, and outages:
  Observe, Orient, Decide (pause for approval), Act, verify, repeat. Use for
  ooda-loop, incidents, latency, throughput, capacity, or cloud spend.
---

# OODA loop — website systems

Loop **Observe → Orient → Decide → (pause) → Act → Observe** until the objective
is met or the user stops. Never skip a phase.

**Observe, Orient, and Decide are read-only.** No edits, deploys, restarts,
scaling, or config/flag changes until Act, and only after the user approves.
Anything that adds load or changes runtime state (load tests, cache warming,
toggling debug logging or sampling) counts as an Act.

## Objective

Before starting, pin one measurable objective the user owns (e.g. "p95 < 300 ms
on /checkout", "cut infra cost 20% without SLO breach", "restore service"). If
unclear, ask once.

## 1. Observe

Collect real, dated, sourced data covering the objective: latency, errors,
saturation, traffic, queue depth, spend, alerts, traces, recent changes. Use
MCPs, vendor CLIs/APIs, observability tools, or local logs. Never invent
numbers; docs and architecture reasoning alone don't count.

**Data gate:** if you can't read live or exported signals, stop and ask for
access by name — MCP/API/CLI for cloud systems, logs/tracing for local ones.

## 2. Orient

Summarize, separating fact, inference, and unknown:

- What is true now vs. the healthy/expected state
- What changed (deploy, traffic, config, dependency)
- Constraints (SLOs, budget, freeze windows, severity)
- Blind spots
- The dominant constraint: latency, errors, capacity, or cost

## 3. Decide — then pause

Propose **one** action, preferring the smallest reversible experiment that
tests the Orient picture:

- **Action**
- **Hypothesis** — which metric improves, by how much
- **Invalidation** — what result means abort
- **Risk / rollback**
- **Tradeoffs** across perf, scale, cost, and reliability

**Stop and wait for explicit approval.** If rejected, re-Orient or re-Decide.
Incidents pause too, unless the user already authorized a specific mitigation.

## 4. Act

Do only the approved action. Capture before/after on the Decide metrics, judge
the outcome (good, mixed, bad) against the hypothesis, and note side effects.
If invalidation trips, roll back and return to Observe; don't chain Acts.

## Loop

- Objective met: stop; report outcome and residual risk
- Otherwise: Observe again with post-Act data, then Orient and Decide (pause)
- Blind spot blocked learning: close the measurement gap before any new Act

## Report (each cycle)

```markdown
## OODA cycle N — [objective]
- Observe: facts (source, window), gaps
- Orient: situation, dominant constraint, blind spots
- Decide: action / hypothesis / invalidation / risk / tradeoffs — approved | amended | rejected
- Act: what ran, metric deltas, outcome, side effects / rollback
- Next: loop | stop | need decision on …
```
