# Concept-First Discovery Playbook: model the domain before the problem

Generic, project-agnostic. Use it to model a product's domain (its entities and their lifecycles) before asking whether any specific feature is worth building. Inject the project specifics (the actual entities, the codebase, the core value or north star) in the "Adaptation to your project" section and in PROJECT_PROFILE.md, not in the core.

What it is: the MOST UPSTREAM track (Track -1), sitting before the Discovery Sequence. Discovery takes a problem as given and asks "is it worth building". This track sits one step earlier and asks "what is the shape of the world we operate in, and what is structurally missing from it". It models the domain as a set of ENTITIES and their LIFECYCLES, maps where ARTIFACTS (notes, records, summaries, logs) attach, and from the gaps produces a list of candidate problems. Those candidates are the input to Discovery.

Why it exists: the most expensive mistake in a product is not bad code, it is building a correct solution for a badly-framed entity. When a request arrives as "merge these two note fields" or "add a comment box" or "make a better history view", the engineering answer (merge the tables, add a column) treats the symptom and leaves the cause: the domain is missing an ENTITY that those artifacts should belong to. Scattered notes are not a notes problem, they are the symptom of a missing entity that would own them. This track finds that missing entity before anyone writes a line of code or even runs a feature-level Discovery.

Relation to the other tracks: Concept-First Discovery produces a domain map and a gap register. Each gap, if it is to be built, passes through the Discovery Sequence (Go/No-Go, see DISCOVERY_PLAYBOOK.md), and after a Go through the Delivery Sequence (see DELIVERY_PLAYBOOK.md). This track has no authority to send anything straight to build: its output is material to validate, not a contract to build. The boundary is hard: this track ends before "is it worth it" (that is Discovery) and long before solution design (that is Delivery).

Status vocabulary: a gap that this track marks REAL becomes an IDEA entry feeding the Discovery Sequence (see STATUS_TAXONOMY.md). Consolidation and DROP findings (housekeeping) skip Discovery and go straight to the backlog as debt cleanup, since they remove rather than add and need no demand validation.

---

# Core method: three distinctions that do the work

## 1. Entity versus artifact
- Entity: has identity and its own lifecycle; it exists over time independently of any single interaction. Test: "does this thing live and change state across many interactions?" Generic examples: customer, order, project, case, patient, account.
- Artifact: a record that is produced at a specific stage of some entity's lifecycle and belongs to it; it has no arc of its own. Test: "what does this belong to, what owns it?" Generic examples: a note, an attachment, a grade, a summary, a status flag, a communication log.
- Consequence: if an artifact has no clear owning entity, then either an entity is missing or the artifact is attached in the wrong place. Most requests of the form "unify the notes" or "add a comment" are the signal of an orphaned artifact.

## 2. Lifecycle as the skeleton
Every entity has: birth, intermediate states and transitions, and closure (or open-ended continuation). Model it as a sequence of states, not a bag of fields. Three arc lengths are worth distinguishing:
- Long arc: the top-level entity (for example a customer or a member, lasting months).
- Medium arc: the intermediate entity (for example a project or a theme: a sequence of interactions with a beginning and an end).
- Short cycle: a single interaction (for example a session or a transaction: before, during, after).
The most common structural gap is a MISSING MEDIUM ARC: the product has a top-level entity and individual interactions, but nothing that binds a run of interactions into one named unit with a closure. That missing middle is exactly why artifacts end up scattered and why continuity is lost between interactions.

## 3. Concept-first: abstraction before code
Model the domain ideally ("how the world works from the user's point of view") BEFORE looking at the code. The reason: code encodes accidental complexity (historical decisions, shortcuts, debt). If you start from the code, you anchor on what IS, and you will never see the missing entity, because by definition it is not in the code. The order is strict: build the abstract model first (lenses 1 through 5), and only then reconcile against the code (lens 6). Breaking the order breaks the method.

---

# Sequence: map

Orchestration throughout: a single lead (Domain Lead). Lenses run in order. Lenses 1 to 5 are abstract (no code). Lens 6 reconciles against the code. Lens 7 attacks the whole.

1. L1 (Entities): enumerate entities and their nesting; separate entities from artifacts.
2. L2 (Lifecycle): map each entity's birth to closure; find empty stages and the missing middle arc.
3. L3 (Artifacts): assign each artifact to one entity and one lifecycle stage; surface orphans, duplicates, dead writes.
4. L4 (Pain): name the user pain per entity and per transition; where continuity breaks.
5. L5 (Value and Gaps): map entities to the core value; produce the gap register (missing entities, open sequences, consolidations, drops).
6. L6 (As-is reconciliation): only now read the code; confirm or correct the model against what exists.
7. L7 (Skeptic): adversary; tries to kill each proposed missing entity (over-modeling, YAGNI).

Gates (binary, blocking):
- Gate A (Model coherence): every item classified entity or artifact; nesting tree coherent; every lifecycle has a beginning and a closure.
- Gate B (Artifact home): every artifact has an owner and a stage, or an explicit orphan / duplicate / DROP status. No artifact left dangling.
- Gate C (Gap justification): every gap has a type, a rank by core value, and survived the Skeptic (REAL, not OVER-MODEL).
- Gate D (Concept-first preserved): the abstract model was built BEFORE reading the code (lenses 1 to 5 before lens 6). If broken, the run is void: repeat.

Recursion: the track is not one-shot. The domain evolves, so the map needs revision when a new product area appears. Re-run on the area that changed, not the whole domain.

---

# Lenses

## L1 (Entities): entities and nesting

Mission: enumerate every entity in the domain and show its nesting or containment, purely abstractly. Separate entities (identity plus lifecycle) from artifacts (records attached to them). This is the foundation; the rest stands on it.

Output spec: a list of entities each with a one-sentence definition (what it is, what gives it identity); a nesting tree (which entity contains which, top-level to intermediate to interaction); a separate list of artifact candidates parked for L3; the entity test applied to each item (lives across many interactions and changes state = entity, otherwise artifact). Bounded context (conditional, not always): when the SAME entity name means materially different things in different parts of the business (classic example: a customer entity that means one thing to billing and another to support), name the bounded context per meaning and either split the entity or tag it with its context. Do this ONLY when the meaning genuinely differs; do not invent a context for every entity.

Definition of Done: every item is explicitly classified entity or artifact (no "it depends"); a nesting tree exists with a single top-level entity (or an explicit statement that the domain has several co-equal roots); entity names are nouns from the user's language, not from the database schema; if any entity name carries two materially different meanings across parts of the business, the contexts are named and the entity is split or context-tagged, otherwise this is explicitly marked not-applicable.

Anti-patterns: mistaking an artifact for an entity (modeling "note" as an entity is almost always wrong); modeling by table names instead of the user's domain language; omitting an entity because it is not in the code (the entire point of this track is to find it); one God entity that means different things to different teams (it should be split by bounded context); the over-correction, inventing contexts where the meaning is actually the same (context explosion, which is itself over-modeling the Skeptic should cut).

Top levers: the entity-versus-artifact test (it dissolves most "merge these fields" requests); naming in the user's language (it reveals entities the schema hid); allowing entities that do not yet exist in code.

## L2 (Lifecycle): lifecycle per entity

Mission: for each entity from L1, draw its lifecycle as a sequence (birth, states and transitions, closure). Name the arc length (long, medium, short) and point at the stages where nothing is currently modeled.

Output spec: a textual state diagram per entity (state to transition to state, with a closure or an explicit "open-ended"); the arc length of each entity; empty stages (lifecycle stages with no representation in the product); the missing-middle test (does a medium-arc entity exist that binds interactions; if not, a red flag for L5). For each transition, name its TRIGGER TYPE: user action / system event / time elapsed / external dependency. Flag transitions gated by something OUTSIDE the user's control (an external system or elapsed time), because that is where continuity stalls and a user gets stuck between states.

Definition of Done: every entity has a lifecycle with an explicit beginning and an explicit closure (or "open-ended"); empty stages are named (where continuity has no carrier); the question "is there a missing medium arc" is answered explicitly; every transition has a named trigger type; externally-gated transitions are flagged as continuity-risk points.

Anti-patterns: a lifecycle modeled as a bag of fields instead of a sequence of states; omitting closure (an entity that never "ends", often the sign of a missing summary); assuming that because the user did not ask for a "before" stage, the "before" stage does not exist; modeling only states and not what drives the transitions (Event Storming: events drive state changes); pulling implementation detail (synchronous versus asynchronous, specific APIs) into this abstract lens, that is a Delivery/architect concern, not Track -1.

Top levers: closure as a first-class stage (it surfaces the missing end-of-arc summary); the arc-length lens (it isolates the missing middle); empty-stage naming (it locates where continuity leaks); the trigger-type lens (it locates the externally-gated transitions where users get stuck).

Reliability boundary: the synchronous-versus-asynchronous classification of a trigger is an implementation property owned by the architect in Delivery, out of scope here. This lens names WHAT drives a transition (user, system, time, external), not HOW it is wired.

## L3 (Artifacts): artifacts and ownership

Mission: take every artifact (notes, grades, summaries, logs, statuses, attachments) and ASSIGN each to one entity plus one stage of that entity's lifecycle. Surface orphans (no owner) and duplicates (several channels for the same thing). Each artifact also carries a retention/privacy class: who may see it, how long it may be kept, and which entity owns its deletion.

Output spec: an assignment table (artifact, owning entity, lifecycle stage, who or what reads it, privacy/retention, status: live / dead / duplicate / orphan); the privacy/retention value is one of None / Confidential / Personal-data (with a retention or deletion rule); orphans (artifacts with no clear entity, candidate missing entities for L5); duplicates (channels holding the same thing, to consolidate); dead writes (artifacts with no reader, which feed no process, candidate DROP).

Definition of Done: every artifact has an owner plus stage, or is explicitly marked an orphan; duplicates are flagged in pairs; every artifact has a named reader, and absence of a reader yields a DROP candidate with a reason; every artifact has a privacy/retention class; every personal-data artifact has a named deletion owner (its owning entity); an ORPHANED personal-data artifact is flagged as a compliance risk (no owner means it is retained indefinitely), not merely a tidiness issue.

Anti-patterns: "this could belong to several entities" (force one primary owner, the rest are references); keeping a dead write because "it might be useful someday" (no reader = debt); failing to notice two artifacts are duplicates because they have different names; treating an orphaned personal-data artifact as only a cleanup item when it is a retention/privacy (for example GDPR) risk.

Top levers: the reader map (it exposes dead writes objectively); one primary owner per artifact; the duplicate scan (it collapses parallel note systems); the deletion-owner lens (the owning entity is what makes deletion/retention enforceable, an artifact with no owning entity has no one to enforce its deletion).

## L4 (Pain): pain per entity and per transition

Mission: for each entity and each lifecycle transition, name the user PAIN: what they hold in their head, what they lose, where they waste time, where continuity breaks between interactions. This lens connects the abstract model to human cost.

Output spec: pain per entity (what hurts in running that entity); pain per transition (where continuity breaks, for example "between interactions there is no recap, I start from zero"); a link back to the empty stages from L2 and the orphans from L3 (pain usually sits exactly where a stage is empty or an artifact is orphaned).

Definition of Done: every entity has at least one concrete named pain (or an explicit "no pain here"); pain is framed as user pain, not as a missing feature; pain is tied to a specific stage or orphan, not generic.

Anti-patterns: pain dressed as a solution ("there is no field for a summary" instead of "I lose the thread between sessions"); pain with no location in the lifecycle (generic "it is inconvenient"); inventing pain from the armchair instead of tying it to an empty stage or orphan just found.

Top levers: tying pain to empty stages (it grounds the gap register); framing as pain not feature (it keeps options open); the transition lens (it finds the lost-continuity pain that per-entity views miss).

## L5 (Value and Gaps): missing entities and sequences

Mission: map the entities and their lifecycles to the product's CORE VALUE (its reason to exist) and produce the GAP REGISTER: missing entities (the missing medium arc above all), open transitions (sequences that should be automatic and are not), and artifacts to consolidate or DROP. This is the product of the whole track.

Output spec: a value map (core value, which entities and transitions carry it, where the loop is open); a gap register (the main artifact), each gap with a type (MISSING ENTITY, OPEN SEQUENCE, CONSOLIDATION, DROP); a ranking of gaps by impact on core value; a downstream-resolved list (things that disappear on their own once the missing entity exists, so they are not built separately); each build-bound gap framed as a candidate PROBLEM ready to enter Discovery (as a pain, not a solution). For each build-bound gap, also state its ARCHITECTURAL CONSTRAINT: the invariant any solution must respect. Example for a MISSING ENTITY gap: "any solution must introduce the entity; a field-level patch on an existing entity does not resolve the structural gap and re-orphans the artifact." This is a CONSTRAINT/invariant, NOT a solution design (designing the solution is still out of scope, that is Delivery).

Definition of Done: every gap has a type; gaps are ranked by impact on core value; downstream-resolved items are listed (anti over-build); every build-bound gap is framed as a candidate problem, not a solution; every build-bound gap states its architectural constraint as an invariant (not a design).

Anti-patterns: designing the solution ("let us build a table with these fields") instead of stopping at "an entity is missing"; pushing a gap straight to build, skipping Discovery's demand validation; omitting downstream-resolved and building separately what the missing entity handles for free.

Top levers: the missing-entity framing (it reframes a pile of feature requests as one architectural gap); downstream-resolved (it prevents redundant builds); ranking by core value (it orders the candidates for Discovery).

## L6 (As-is reconciliation): confront the code

Mission: ONLY NOW look at the code (the entity list, where artifacts actually persist, what is dead) and reconcile it against the ideal model from L1 to L5. Differences confirm the gaps (or correct the model). This is where a code-reading fan-out (parallel readers with file:line citations) plugs in.

Output spec: a model-versus-code table (entity or artifact from the model, representation in code with file:line or "absent", verdict: confirmed gap / exists / model correction); confirmations (which L5 gaps the code confirms hard); model corrections (what the code revealed that the model lacked); false alarms removed (things that looked like a gap but exist, for example a field inherited from a base class).

Definition of Done: every entity or artifact from the model has a verdict grounded in real code (file:line), not memory; contradictions between sources are resolved by reading the source, not by voting; the model is updated with the code corrections.

Anti-patterns: starting from the code (breaks concept-first, see Core method, distinction 3); taking a reader's conclusion as fact without checking the source on a contradiction; confusing "the code does not read this artifact" (a dead write, a real signal) with "users do not use this" (which needs telemetry, not code).

Top levers: the strict order (the abstract model first makes the reconciliation a confirmation, not an anchor); file:line grounding (it kills cross-reader contradictions); the reader-versus-usage boundary (it keeps dead-write claims honest).

Reliability boundary: the code tells you what EXISTS and what is READ by the system. It does not tell you how often a user actually uses something: that needs analytics or telemetry and is out of this track's scope.

## L7 (Skeptic): red-team

Mission: try to KILL each proposed missing entity and each gap. The most common modeling error is over-engineering: inventing structure the domain does not need. The Skeptic defaults to "do not add the entity" and forces the rest to overturn that.

Output spec: an attack on each missing entity (does the domain need it, or do we just want clean architecture); an attack on entity boundaries (is the split or merge correct, one entity versus two); an attack on gaps (does the pain justify the structure, or would a smaller move, an artifact on an existing entity, suffice); a "does it already exist" recon (do not invent a gap that is already filled); a verdict per gap (REAL = stays, OVER-MODEL = strike, DOWNGRADE = an artifact suffices, not an entity).

Definition of Done: every missing entity got a concrete attack (not a generic one); YAGNI checked (would a simpler move, an artifact on an existing entity, suffice); the "does it already exist" recon done; a REAL / OVER-MODEL / DOWNGRADE verdict per gap.

Anti-patterns: token skepticism (it passes every pretty entity); no YAGNI attack (the model bloats with entities nobody needs); skipping the recon (the register contains things already built).

Top levers: default "do not add the entity" (it reverses the burden of proof); the existence recon (it deletes already-built gaps); the downgrade option (artifact instead of entity is often enough).

---

# Output template: Domain Map plus Gap Register (handoff to Discovery)

```
## Domain map: <domain or area name>
Date: <YYYY-MM-DD>

ENTITIES (entity | definition | arc length | contains):
- <top-level entity> | <def> | long   | <intermediate entities>
- <intermediate>      | <def> | medium | <interactions>      [<-- if ABSENT: the missing middle]
- <interaction>       | <def> | short  | -

LIFECYCLES (entity: state -> ... -> closure | empty stages):
- <entity>: <birth> -> <state> -> <closure>  | EMPTY: <stage with no carrier>

ARTIFACTS (artifact | owner | stage | reader | privacy/retention | status):
- <artifact> | <entity> | <stage> | <who reads it> | <None / Confidential / Personal-data + rule> | live/dead/duplicate/orphan

## Gap register (ranked by core value)
| # | Gap | Type | Impact on core value | Architectural constraint | Skeptic verdict | Candidate for Discovery? |
|---|-----|------|----------------------|--------------------------|-----------------|--------------------------|
| 1 | <missing entity X> | MISSING ENTITY | <how it closes the loop> | <invariant Delivery must honor, e.g. must introduce the entity, not a field-level patch> | REAL | YES, problem: "<user pain>" |
| 2 | <auto transition Y> | OPEN SEQUENCE | <...> | <invariant> | REAL | YES |
| 3 | <duplicate A/B>     | CONSOLIDATION | <...> | -            | REAL | no (housekeeping) |
| 4 | <dead write Z>      | DROP | <...> | -            | REAL | no (remove) |

DOWNSTREAM-RESOLVED (disappears once the missing entity exists):
- <thing NOT built separately because the missing entity handles it>

MODEL CORRECTIONS FROM CODE (L6):
- <what the code revealed or removed>
```

Each "Candidate = YES" row enters the Discovery Sequence as a loose problem (the signals or opportunity inbox), where it gets an outcome, a demand signal, a RAT, and a Go/No-Go. Consolidations and DROPs go straight to the housekeeping backlog (they need no demand validation, they pay down debt). The architectural constraint travels WITH the candidate problem into Discovery's Brief and must be honored by Delivery, so that a validated problem is not later solved with a field-level patch that recreates the gap (the intent here, not an edit to DISCOVERY_PLAYBOOK.md or DELIVERY_PLAYBOOK.md).

---

# When to run this track (triggers)

- The request is framed "engineering-low": "merge these notes", "add a comment field", "make a better history view". Suspect an orphaned artifact or a missing entity.
- Artifacts are scattered with no clear owner (several kinds of note or record doing similar things).
- Before a large architectural decision that touches many features at once (a migration, a data-model refactor).
- Entering a new product area whose domain has not been modeled yet.
- You sense that "the value loop only closes halfway": the classic symptom of a missing medium arc.

When NOT to run it: a single well-defined feature in an already-modeled domain (go straight to Discovery). A small fix or bug (go straight to Delivery, or just do it).

---

# Continuous loop

This track is not a one-time gate. The domain map is a living artifact. Re-run a lens pass when a new area appears, when a recurring "merge these fields" request signals an orphaned artifact, or when a build keeps tripping over the same missing middle. Keep the map cheap to maintain: the product is a map and a list, not code, so a pass is hours to a day or two, not weeks.

---

# Anti-patterns of the whole track

- Code-first instead of concept-first: starting from the schema guarantees you will not see the missing entity.
- Over-modeling: adding entities for architectural elegance, not for domain pain (the Skeptic exists to cut this).
- Track confusion: designing the solution or validating demand in this track (that is Discovery and Delivery). This track ends at "an entity is missing / a gap exists".
- Artifact as entity: modeling a note or comment as a first-class entity instead of a record attached to one.
- Pushing to build skipping Discovery: the gap register is candidates to validate, not a build order.
- Omitting downstream-resolved: building separately what the missing entity handles for free (for example "merging the notes" as its own project).

---

# Adaptation to your project

Keep this playbook generic. Put the project specifics in PROJECT_PROFILE.md and reference them here:
- The actual entities and their names in the user's language (the domain nouns).
- The core value or north star the gap register ranks against.
- The code locations for L6 (the entity directory, the persistence layer, the migration history), and whether a code-reading fan-out is available.
- The hard rules and footgun register your stack imposes once a gap reaches Delivery (kept in PROJECT_PROFILE.md, not here).
- The reliability boundary for your evidence: code tells you what exists and what is read; usage frequency needs telemetry, which you note as a separate source if available.

Output of this track (the concrete domain map and gap register for your product) is a RESULT: keep it in your project, not in this template repo. This repo stays a clean, generic source.
