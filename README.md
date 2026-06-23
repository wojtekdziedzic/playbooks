# Playbooks: a generic product process (Discovery + Delivery)

A project-agnostic template for taking any idea from "should we build this and what exactly" through "how to build it well", regardless of the project. It packages two role sequences plus a state layer so a cold reader can triage, decide, and deliver without reinventing the process each time.

This repo is the SOURCE (a template), not the result of any one analysis. You keep the specifics of a concrete project (outcome, hard rules, stack, personas, deadline) in a separate sidecar file, `PROJECT_PROFILE.md`, and leave these playbooks clean and shared across projects.

## Two tracks plus a state layer

```
IDEA
  -> [Track 0] Discovery Sequence: should we build it, and what
       (8 roles + 5 gates -> Go/No-Go + Brief)
  -> [Track 1] Delivery Sequence: how to build it well
       (BA -> SA -> Tech Lead -> Backend -> Frontend -> QA -> Security)
  -> release

   [state layer] Status Taxonomy: where each idea sits in the flow
```

The two tracks form a pipeline: each role's output is the next role's input gate. Discovery's Go produces a Brief, and that Brief is the Backlog Analyst's (BA) input on the Delivery side. The Status Taxonomy is orthogonal to both: it tells you, at any moment, which state an idea is in so the backlog never collapses into a single undifferentiated "PARKED".

## Files

- [DISCOVERY_PLAYBOOK.md](DISCOVERY_PLAYBOOK.md): the Discovery Sequence (the upstream "should we and what to build"). 8 roles: PDL (Product Discovery Lead), PS (Product Strategist), UXR (User Researcher), MKT (Market and Competition Analyst), PRC (Pricing and Behavioral Analyst), DATA (Analytics), EXP (Experiment Designer), SKEPTIC (Red-team). 5 gates: Outcome, Evidence, Demand, Worth-it, Go/No-Go. Output: a Brief handed to Delivery, or a No-Go with the condition under which it would be revisited.
- [DELIVERY_PLAYBOOK.md](DELIVERY_PLAYBOOK.md): the Delivery Sequence (the downstream "how to build it well"). 7 roles: BA -> SA -> Tech Lead -> Backend -> Frontend -> QA -> Security. Plus Cross-cutting rules (lead with the point, recon gate, reuse first, decisions stated with a criterion and a trigger, footgun register, Definition of Done per role, release pre-flight) and Gates. Input: the Brief from Discovery (a Go).
- [STATUS_TAXONOMY.md](STATUS_TAXONOMY.md): the status of an idea across both sequences (IDEA, DISCOVERY, RAT, READY, DELIVERY, STAGING, SHIPPED, plus PARTIAL and VERIFY, plus PARKED, BLOCKED, KILLED) and a route dimension. Route D->E means the idea needs Discovery first and then Delivery. Route E means demand is already certain, so Discovery is skipped and the idea goes straight to Delivery. Route D means Discovery only (research, no build committed yet). This is the cure for a backlog drowned in one ambiguous "PARKED".
- [EXAMPLE_DISCOVERY.md](EXAMPLE_DISCOVERY.md): an anonymized, worked Discovery example showing the full role-by-role flow through the gates to a Brief. Use it as a reference for what good Discovery output looks like.
- [.gitattributes](.gitattributes): line-ending normalization so the template stays clean across machines.

## Shared anatomy of a role entry

Every role in both playbooks uses the same canonical section vocabulary, so once you know one role you can read any of them: Mission, Output spec, Definition of Done, Anti-patterns, Top levers, Gates, Cross-cutting rules, and Adaptation to your project. Reading them in that order tells you what the role is for, what it must produce, when it is done, how it fails, and where the leverage is.

## Boundary

Discovery ends BEFORE solution design begins. Discovery says "it is worth solving problem P for persona [persona], because [reason]". Delivery says "here is how we will solve it". Nothing enters Delivery without a Go from Discovery, or an explicit skip under the Route E rule in the taxonomy.

## How to use in your own project

1. Clone the repo (or copy the two playbooks, the taxonomy, and the example).
2. Create your own `PROJECT_PROFILE.md` with the specifics: outcome / north star, hard rules, the footgun register for your stack, the release pre-flight checklist, your design system, and your personas. `PROJECT_PROFILE.md` is the canonical sidecar for everything project-specific.
3. Run your analyses and builds, keeping the RESULTS (briefs, plans, backlog refactors) in YOUR project, not here. This repo stays a clean source.

Suggested reading order for a first-time user: README -> STATUS_TAXONOMY.md (to triage where an idea is) -> DISCOVERY_PLAYBOOK.md or DELIVERY_PLAYBOOK.md depending on the idea's route.

## Writing convention

English, plain ASCII only, straight quotes only (" and '). No em-dashes or en-dashes anywhere: use commas, colons, parentheses, or periods instead. For a label and its description, use a colon or parentheses (for example "PS (Product Strategist)" or "PS: Product Strategist"), never a dash or a dash substitute.
