# Metrics Playbook: how to choose good metrics

Generic and project-agnostic. Use it whenever a metric is being chosen: the project's north star, a per-feature success metric in Discovery or Delivery, or a RAT threshold. Inject the project specifics (the actual north star, the lever set, the tooling) in the "Adaptation to your project" section and in `PROJECT_PROFILE.md`, never into the core.

Why: the other playbooks demand metrics everywhere (PS attaches problems to an outcome, DATA pre-registers thresholds, BA writes baseline -> target -> window, post-release verification closes the loop) but none of them says how to pick a GOOD metric. A bad metric is worse than none: it green-lights weak ideas, hides real failures, and gets gamed. This file is the shared standard those roles reference.

Relation to the other files: `DISCOVERY_PLAYBOOK.md` (PS outcome link, DATA baseline and thresholds, EXP decision rules), `DELIVERY_PLAYBOOK.md` (BA hypothesis and success metric, post-release verification), `STATUS_TAXONOMY.md` (the Outcome metadata every live item must carry, and the VERIFY recon that measures the metric).

---

## 1. The metric stack (four layers, top down)

```
NORTH STAR        one metric for the whole product, declared in PROJECT_PROFILE.md
  -> LEVERS       the handful of drivers the north star decomposes into
                  (example set: acquisition, activation, retention, revenue,
                  compliance, debt; swap for your own)
    -> FEATURE SUCCESS METRIC   per idea or feature: baseline -> target -> window
      -> COUNTER-METRIC         per success metric: what must NOT get worse
```

Every layer attaches upward: a feature metric that maps to no lever, or a lever that does not plausibly move the north star, has no right to exist. This is the same rule as "an idea without a lever has no right to take a slot" in `STATUS_TAXONOMY.md`, applied to numbers instead of ideas.

## 2. Choosing the north star

One metric, not a dashboard. Criteria, all required:

- **Value delivered, not value extracted.** It measures the user receiving value (bookings completed, documents processed, active teams), not the business skimming it (revenue is a consequence, rarely a good north star on its own).
- **Recurring, not cumulative.** A counter that can only go up (total signups ever) hides decline. Use a rate or an active-in-window form.
- **Movable by the team.** If no plausible feature changes it within a quarter, it is a board-slide number, not a north star.
- **Hard to game.** Ask "what is the laziest way to inflate this without helping anyone" and check the answer is either absurd or caught by a guardrail.

Declare the north star and its lever decomposition in `PROJECT_PROFILE.md`. Revisit on strategy change, not per feature.

## 3. Choosing a feature success metric

The form is fixed by the other playbooks: **baseline -> target -> window**, binary checkable. This file adds the quality bar for each part:

- **Baseline** comes from a real source (analytics, logs, tracker), never from memory or a guess. No measurable baseline = instrument first, or state explicitly that the baseline is zero because the capability does not exist.
- **Target** states its mechanism: WHY this feature moves this number by this much. A target with no mechanism is a wish. Sanity-check the size: a 50 percent jump needs an extraordinary mechanism.
- **Window** is long enough to see the behavior (at least one full usage cycle of your product) and short enough to act on; state the check date, and name who checks it (that is the post-release verification owner in `DELIVERY_PLAYBOOK.md`).
- **Binary checkable**: on the check date, a cold reader can answer met / not met from the named source with no interpretation.

## 4. Counter-metrics

Every success metric carries at least one counter-metric: the number that detects the cheap, harmful way to hit the target. Choose it by asking "if we hit the target by doing damage, where does the damage show up first". Examples of the pattern (generic): pushing signups can degrade activation; pushing speed can degrade error rate; pushing engagement can degrade task completion. A success metric without a counter-metric is unfalsifiable success.

## 5. Leading vs lagging, and proxies

- **Lagging** metrics (retention, revenue) prove outcomes but move slowly. **Leading** metrics (activation events, first-use latency) move fast but only correlate. Pair them: lead with a leading metric for the window, confirm with the lagging one at the next horizon.
- A **proxy** metric (clicks on a fake-door tile as a proxy for demand) is legal ONLY with two things stated up front: the causal link (why the proxy predicts the real thing) and the expiry (when you will re-measure the real thing directly). A proxy without an expiry silently becomes the goal: that is Goodhart's law, and the counter-metric plus expiry are the two defenses.

## 6. The five-question vanity test

Run every candidate metric through these; any NO means pick another metric or fix the definition:

1. **Decision test**: name the decision that changes if this number moves. None = vanity.
2. **Down test**: can it go down, and would you see it within the window? A metric that can only rise measures accumulation, not health.
3. **Denominator test**: is it a rate with a defined population (who is in, who is out), not a raw count?
4. **Gaming test**: the laziest inflation path is either absurd or caught by the counter-metric.
5. **Owner test**: one named person checks it on a named date from a named source.

## 7. Pre-registration (the contract with tests)

Thresholds are set BEFORE the measurement, never after. This repeats the DATA -> EXP contract from `DISCOVERY_PLAYBOOK.md` because it is the single most violated rule: DATA pre-registers the RAT threshold, EXP consumes it unchanged, and the persevere / pivot / kill rule is written before the test starts. Moving a threshold after seeing the data is not analysis, it is rationalization. The same applies in Delivery: BA's target is fixed before build, and post-release verification checks against THAT target, not a revised one.

## 8. Anti-patterns

- A dashboard of twenty numbers instead of one north star with levers.
- A cumulative counter (total users ever) presented as health.
- A target with no baseline ("increase engagement").
- Success declared on the success metric while the counter-metric quietly degraded (or no counter-metric existed).
- A proxy that outlived its expiry and became the goal.
- A threshold adjusted after the result was known.
- A metric nobody owns: it gets computed once for the launch announcement and never again.

## 9. Ownership

- The PO owns the north star and the lever set (declared in `PROJECT_PROFILE.md`).
- DATA (Discovery) owns baselines, instrumentation, and pre-registered thresholds for RATs.
- BA (Delivery) owns the feature success metric and counter-metric in the Brief-to-build handoff.
- The post-release verification owner (assigned per `DELIVERY_PLAYBOOK.md`, often Tech Lead or Backend) owns the check on the named date. This is what closes the loop and feeds the VERIFY recon in `STATUS_TAXONOMY.md`.

---

## Adaptation to your project

Keep the specifics in `PROJECT_PROFILE.md`, leave this core clean:

- **North star and levers**: declare the one metric and its decomposition; swap the example lever set for your own.
- **Sources of truth**: name where baselines and checks come from (your analytics stack, logs, tracker).
- **Usage cycle**: state your product's natural cycle length so windows are sized against it (daily-use tool vs monthly workflow differ by an order of magnitude).
- **Standard counter-metrics**: if your domain has recurring ones (error rate, support tickets, churn), list them so features pick from a known menu.
- **Check cadence**: fold the post-release metric check into an existing ritual (weekly review, sprint review) so the owner test always has a date.
