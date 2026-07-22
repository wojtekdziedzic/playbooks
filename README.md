# Playbooks: a generic product process (Concept-First + Discovery + Delivery)

A project-agnostic template for taking any idea from "what is the shape of the domain and what is missing from it" through "should we build this and what exactly" to "how to build it well", regardless of the project. It packages three tracks plus a state layer so a cold reader can model, triage, decide, and deliver without reinventing the process each time.

This repo is the SOURCE (a template), not the result of any one analysis. You keep the specifics of a concrete project (outcome, hard rules, stack, personas, deadline) in a separate sidecar file, `PROJECT_PROFILE.md`, and leave these playbooks clean and shared across projects.

## Three tracks plus a state layer

```
DOMAIN (entities + lifecycles)
  -> [Track -1] Concept-First Discovery: what is the domain, what entity is missing
       (7 lenses + 4 gates -> domain map + gap register)
IDEA (a candidate problem, often a gap surfaced above)
  -> [Track 0] Discovery Sequence: should we build it, and what
       (8 roles + 5 gates -> Go/No-Go + Brief)
  -> [Track 1] Delivery Sequence: how to build it well
       (BA -> SA -> Tech Lead -> Backend -> Frontend -> QA -> Security)
  -> release

   [state layer] Status Taxonomy: where each idea sits in the flow
```

The tracks form a pipeline: each stage's output is the next stage's input gate. Concept-First Discovery turns a domain into ranked gaps; a gap marked REAL becomes an IDEA feeding Discovery. Discovery's Go produces a Brief, and that Brief is the Business Analyst's (BA) input on the Delivery side. The Status Taxonomy is orthogonal to all three: it tells you, at any moment, which state an idea is in so the backlog never collapses into a single undifferentiated "PARKED".

Two support playbooks cut across the tracks: the Metrics Playbook (how to choose the numbers every track demands) and the Dependencies Playbook (how to coordinate across team boundaries). They are not tracks; they are shared standards the track roles reference.

## Files

- [CONCEPT_FIRST_DISCOVERY_PLAYBOOK.md](CONCEPT_FIRST_DISCOVERY_PLAYBOOK.md): the Concept-First Discovery track (Track -1, the most upstream "what is the domain and what entity is missing"). 7 lenses: Entities, Lifecycle, Artifacts, Pain, Value and Gaps, As-is reconciliation, Skeptic. 4 gates: Model coherence, Artifact home, Gap justification, Concept-first preserved. Core idea: entity versus artifact, lifecycle as the skeleton, abstraction before code. Output: a domain map plus a gap register (missing entity, open sequence, consolidation, drop); a gap marked REAL becomes a candidate problem feeding Discovery. Sources: Domain-Driven Design, Event Storming.
- [DISCOVERY_PLAYBOOK.md](DISCOVERY_PLAYBOOK.md): the Discovery Sequence (the upstream "should we and what to build"). 8 roles: PDL (Product Discovery Lead), PS (Product Strategist), UXR (User Researcher), MKT (Market and Competition Analyst), PRC (Pricing and Behavioral Analyst), DATA (Analytics), EXP (Experiment Designer), SKEPTIC (Red-team). 5 gates: Outcome, Evidence, Demand, Worth-it, Go/No-Go. Output: a Brief handed to Delivery, or a No-Go with the condition under which it would be revisited.
- [DELIVERY_PLAYBOOK.md](DELIVERY_PLAYBOOK.md): the Delivery Sequence (the downstream "how to build it well"). 7 roles: BA -> SA -> Tech Lead -> Backend -> Frontend -> QA -> Security. Plus Cross-cutting rules (lead with the point, recon gate, reuse first, decisions stated with a criterion and a trigger, footgun register, Definition of Done per role, release pre-flight) and Gates. Input: the Brief from Discovery (a Go).
- [STATUS_TAXONOMY.md](STATUS_TAXONOMY.md): the status of an idea across the tracks (IDEA, DISCOVERY, RAT, READY, DELIVERY, STAGING, SHIPPED, plus PARTIAL and VERIFY, plus PARKED, BLOCKED, KILLED) and a route dimension. Route D->E means the idea needs Discovery first and then Delivery. Route E means demand is already certain, so Discovery is skipped and the idea goes straight to Delivery (assigned via an explicit decision tree, including a fast path for trivial fixes of at most 4 hours). Route D means Discovery only (research, no build committed yet). PARKED carries a revival trigger, a review date, and a max park time (default 90 days). This is the cure for a backlog drowned in one ambiguous "PARKED".
- [METRICS_PLAYBOOK.md](METRICS_PLAYBOOK.md): how to choose good metrics. The metric stack (north star -> levers -> feature success metric -> counter-metric), criteria for a north star, the quality bar for baseline -> target -> window, counter-metrics, leading vs lagging and proxies with expiry, the five-question vanity test, pre-registration of thresholds, and ownership. Feeds PS and DATA in Discovery, BA and post-release verification in Delivery.
- [DEPENDENCIES_PLAYBOOK.md](DEPENDENCIES_PLAYBOOK.md): cross-team coordination. The Dependency Register (one row per dependency with owners, need-by, and fallback), dependency types (blocking / soft / informational), written contracts before integration, backward-computed need-by dates, the requested -> committed -> delivered -> verified handshake, the fallback rule, and the escalation ladder. Gives the BLOCKED status its operating procedure and feeds the Tech Lead's integration contracts.
- [EXAMPLE_DISCOVERY.md](EXAMPLE_DISCOVERY.md): an anonymized, worked Discovery example showing the full role-by-role flow through the gates to a Brief. Use it as a reference for what good Discovery output looks like.
- [PROJECT_PROFILE_TEMPLATE.md](PROJECT_PROFILE_TEMPLATE.md): the fill-in template for your project's `PROJECT_PROFILE.md` sidecar. One section per consumer group (outcome and levers, domain, personas and signals, metrics rules, hard rules, footgun register, delivery conventions, security and compliance, release pre-flight, status operations, cross-team coordination), each field annotated with which role or gate consumes it and which playbook defaults it may override.
- [.gitattributes](.gitattributes): line-ending normalization so the template stays clean across machines.

## Shared anatomy of a role entry

Every role in the Discovery and Delivery playbooks uses the same canonical section vocabulary, so once you know one role you can read any of them: Mission, Output spec, Definition of Done, Anti-patterns, Top levers, Gates, Cross-cutting rules, and Adaptation to your project. Reading them in that order tells you what the role is for, what it must produce, when it is done, how it fails, and where the leverage is.

## Boundary

Discovery ends BEFORE solution design begins. Discovery says "it is worth solving problem P for persona [persona], because [reason]". Delivery says "here is how we will solve it". Nothing enters Delivery without a Go from Discovery, or an explicit skip under the Route E rule in the taxonomy.

## How to use in your own project

1. Clone the repo (or copy the three playbooks, the taxonomy, and the example).
2. Copy [PROJECT_PROFILE_TEMPLATE.md](PROJECT_PROFILE_TEMPLATE.md) into your project as `PROJECT_PROFILE.md` and fill in the placeholders: outcome / north star, hard rules, the footgun register for your stack, the release pre-flight checklist, your design system, and your personas. `PROJECT_PROFILE.md` is the canonical sidecar for everything project-specific.
3. Run your analyses and builds, keeping the RESULTS (briefs, plans, backlog refactors) in YOUR project, not here. This repo stays a clean source.

Suggested reading order for a first-time user: README -> STATUS_TAXONOMY.md (to triage where an idea is) -> CONCEPT_FIRST_DISCOVERY_PLAYBOOK.md (when you need to model the domain before any specific idea) -> DISCOVERY_PLAYBOOK.md or DELIVERY_PLAYBOOK.md depending on the idea's route. Read METRICS_PLAYBOOK.md when you set up the metric stack (or the first time any role has to pick a number), and DEPENDENCIES_PLAYBOOK.md the first time a build crosses a team boundary.
