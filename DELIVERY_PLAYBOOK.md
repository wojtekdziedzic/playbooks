# Delivery Sequence Playbook: maximize every role

Generic and project-agnostic. Take it and apply it to any software project. You inject your project specifics (hard rules, stack, personas, deadline) in the "Adaptation to your project" section at the end, never into the playbook core. Keep the core clean and shared across projects.

What it is: a way to run the BUILD of a feature through a sequence of 7 roles (BA: Business Analyst, SA: Solution Architect, Tech Lead, Backend, Frontend, QA, Security) so that each operates at a world-class level and the whole does not lose quality or burn time. This is the "HOW to build it well" track (track 1).

Relation to Discovery: Delivery assumes it is already decided that you are building. The input is the Brief from the Discovery Sequence (Go); see DISCOVERY_PLAYBOOK.md. If you have no Discovery, the input is a validated problem from a decision-maker. Nothing without a validated problem enters here. For the status vocabulary used around this track (READY, DELIVERY, STAGING, SHIPPED, PARTIAL, VERIFY, PARKED, BLOCKED, KILLED), see STATUS_TAXONOMY.md. Only an idea in READY enters DELIVERY.

Scaling down: the 7 roles are responsibilities, not headcount. On a small team one person can play several roles in sequence (for example BA plus SA plus Tech Lead as one pass, then implementation). Collapse or merge roles freely, but do not skip the output each role owns: the artifacts and gates below still apply even when one person produces all of them.

---

# Cross-cutting rules (apply to every role)

1. **Lead with the point (decision TL;DR).** Every artifact opens with a block: the problem in 1-2 sentences, the default recommendation, a small number of decisions (cap them, for example at most three) each marked [rec.], and an explicit "default-on-OK" (what happens if the decision-maker says nothing). The decision-maker is oriented at the top, not after reading the whole document.
2. **Recon gate (before planning).** Before you design anything, check what ALREADY exists in the code and product. Do not rebuild what is there. The recon verdict (exists / partial / absent) is a precondition that opens the architect's work; it is captured by SA as the first output, so recon both precedes and is owned by SA. Without recon you risk spending a week on something already shipped.
3. **Reuse first.** First the list of "reuse as is" and "reuse with modification", only then "new". Close with the line "net new: N files / M endpoints" as input to estimation. Reuse is the main lever for keeping cost low.
4. **Decisions A vs B with explicit criterion and revision trigger.** Every decision: pros and cons per option, at least one option explicitly REJECTED with a reason, a "Decision criterion" line (the axis of choice), a "Recommendation", and a "Revision trigger" (a measurable condition that brings the rejected option back). A decision must be reproducible.
5. **Footgun Register.** The project maintains its own list of runtime traps in its stack (things the compiler and types do NOT catch but that blow up in production). SA checks them off per feature (applies YES / NO / N.A. plus mitigation). The list grows after every incident. This replaces reliance on memory. The concrete entries live in PROJECT_PROFILE.md, not here.
6. **Definition of Done per role is a gate.** A role's output is the next role's input. A missing element of the Definition of Done blocks the handoff. Quality caught early is the cheapest.
7. **Release pre-flight gate.** One binary gate before release that ties together all of the project's release conditions (see PROJECT_PROFILE.md). Absent any of them: do not release.
8. **Iteration is allowed (loops, not only forward).** The default flow is forward (BA to SA to Tech Lead to Backend to Frontend to QA to Security), but delivery loops back when a later role finds a gap. The backward path is explicit: if QA finds an acceptance-criterion ambiguity, it returns to BA; if Backend finds the architecture cannot satisfy an AC, it returns to SA. A loop back re-opens that role's Definition of Done; the corrected output flows forward again. This backward movement mirrors STATUS_TAXONOMY.md, where an item can move back to an earlier state (for example STAGING back to DELIVERY on a QA failure), not only forward.

If you execute the build with LLM agents: match model strength to the risk of each phase (simple or mechanical work uses a cheaper model; money, security, or sensitive-data work uses the strongest). Do not average effort across the whole document. This is an optional adaptation, not a requirement for human-only teams.

---

# Artifact and traceability convention

The roles below list WHAT to produce. Use a single living spec as the carrier document for one feature, with one section per role, rather than disconnected per-role files. Traceability propagates through it: BA assigns IDs to user stories (US-1, US-2) and acceptance criteria (US-1-AC-1); SA, Tech Lead, Backend, and QA all reference those IDs so any line can be traced from test back to AC back to user story. Each role's Output spec includes a minimal fill-in shape (a skeleton) so the artifact is usable cold.

Estimation convention: pick one unit for the whole feature and state it (for example ideal engineer-days, or story points with a stated points-to-days mapping, or t-shirt sizes S/M/L with a stated range). Tech Lead fixes the unit at the top of the plan so all estimates are comparable.

---

# Sequence of roles

## BA (Business Analyst)

**Mission:** Turn a loose problem into a binary-testable contract: a named persona plus job-to-be-done, a value hypothesis with a numeric baseline and target, atomic acceptance criteria, traceability from user stories to AC, and an out-of-scope that protects against scope creep. BA is the first gate for the project's hard rules.

**Output spec:**
- Problem and value: 1-2 sentences of the problem grounded in a real signal or code (not a guess); a named persona; JTBD "When [situation], I want [motivation], so that [outcome]".
- Hypothesis and success metric: numeric baseline -> target -> time window; a counter-metric (what must NOT get worse).
- User stories: As-a / I-want / So-that, numbered, marking what is outside the MVP.
- Acceptance criteria: Given / When / Then schema, hard thresholds and numbers (no "e.g.", "etc.", "readable", or an undefined "N"; use a concrete value or "a small number"); an explicit error code for domain validation.
- Process flow: happy path plus edge states (already done / error / expired / skipped) plus entry points.
- Guardrail check: a mini-checklist of the project's hard rules (YES / NO / N.A. with justification). BA gates these AT THE ENTRY.
- Traceability: a table US -> AC -> in-scope; zero orphaned user stories.
- Decisions for the decision-maker, separated: (A) strategic or irreversible with a recommendation; (B) default thresholds already written into the AC (the decision-maker only vetoes).

Skeleton: `Problem | Persona | JTBD | Metric (baseline -> target -> window) | Counter-metric | US-n + AC-n (G/W/T) | Out-of-scope | Guardrail check | Traceability table | Open decisions`.

**Definition of Done:** persona plus JTBD present; metric with baseline, target, and window; every AC binary (Given / When / Then, no soft words); guardrail check filled in; traceability table without orphans; only real strategic decisions left open (thresholds have defaults).

**Anti-patterns:** a generic actor with no persona; a metric with no numbers; AC with "e.g." or "N"; a user story with no AC and no out-of-scope; pushing enforcement of hard rules onto later roles; mixing real decisions with missing definitions.

**Top levers:** persona plus JTBD as the foundation, because every downstream role builds on who and why; a metric with a baseline, because without it nobody can close the loop post-release; the guardrail check at entry, because hard rules are cheapest to enforce before any design exists; atomic AC, because they are the unit QA tests 1:1.

## SA (Solution Architect)

**Mission:** Turn the problem into one architectural decision ready to accept without a round of clarifications: recon (what already exists), maximum reuse, defusing the project's known runtime footguns, and the project's product gates (injected from PROJECT_PROFILE.md, for example device parity, accessibility, acceptance of AI artifacts). A reproducible decision: each option with a criterion and a revision trigger.

**Output spec:**
- Recon: a table symbol/endpoint -> where you looked -> verdict (exists / partial / absent); a one-sentence verdict on whether the feature or part of it already exists. Recon is the precondition from cross-cutting rule 2 and SA owns its capture.
- Reuse first: "reuse as is" plus "reuse with modification" plus "net new: N".
- Decisions Dx (A vs B): pros and cons, the rejected option with a reason, criterion plus recommendation plus revision trigger.
- "Minimal surface for v1" decision: before adding any new persistent state, new module, or new external dependency, you explicitly weigh the lean variant; the heavier path goes to phase 2 with a trigger.
- Footgun guard: a table from the project's Footgun Register, per entry YES / NO / N.A. plus mitigation.
- Project product gates: the guardrails injected from PROJECT_PROFILE.md, checked off here.
- Component table: New / Mod / Reuse, consistent with recon (nothing marked "New" that recon found).

Skeleton: `Recon table | One-line verdict | Reuse lists + net new | Decisions Dx (criterion/rec/trigger) | Minimal-surface decision | Footgun guard table | Product gates | Component table`.

**Definition of Done:** recon done; zero contradictions between recon and components; footgun guard checked off; every decision has a criterion, a trigger, and a rejected option; minimal surface considered.

**Anti-patterns:** designing without recon; "new" for something that exists; a decision with no criterion or trigger; skipping footguns; an excess new persistent entity where an extension would do.

**Top levers:** the recon gate, because it prevents rebuilding shipped work; reuse first, because reuse is the cheapest path to delivery; explicit criterion plus trigger, because it makes the decision reproducible and auditable; the footgun guard, because it catches what types and the compiler cannot.

## Tech Lead

**Mission:** Break the SA decision into a sequence of executable steps with estimates and ordering, name implementation risks and integration points, and define the release pre-flight. The Tech Lead owns "how to wire it together, in what order, and what can go wrong".

**Output spec:**
- Step plan with dependencies and an estimate per step, in the fixed estimation unit (see the artifact convention above).
- Ordering: what unblocks what, expressed as a simple dependency notation (for example step IDs with "depends on").
- Risks plus mitigations: each named implementation or integration risk with a concrete mitigation.
- Integration contracts: APIs, events, and shared types defined up front so parallel work does not diverge.
- Release pre-flight checklist for this feature: the concrete subset of the project's release conditions (from PROJECT_PROFILE.md) that this feature touches.
- Explicit reuse with names from the code (what existing symbols the plan reuses).

Skeleton: `Estimation unit | Step list (id, action, estimate, depends-on) | Critical path | Risk table (risk -> mitigation) | Integration contracts | Pre-flight checklist | Reuse by name`.

**Definition of Done:** steps atomic with estimates; the critical path is clear; risks named with mitigations; the pre-flight checklist is complete.

**Anti-patterns:** a plan with no ordering or estimate; silent integration risks; no pre-flight; an estimate that ignores reuse.

**Top levers:** the critical path, because it gives the decision-maker the one number that matters (days to release); integration contracts up front, because they let parallel tracks proceed without rework; the pre-flight as a gate, because it is the single point that catches release-blocking gaps.

## Backend

**Mission:** Implement the domain logic and data per the AC, with correct error codes, idempotency where it matters (any operation that must not double-apply, for example money movements or external-event handlers), and no known data or migration footguns.

**Output spec:**
- Endpoints and services per the AC, traced to the AC IDs.
- Input validation with the correct error code (do not confuse authorization with validation; never return an auth error for a domain-validation failure).
- Idempotency for sensitive operations (a guard plus a unique key) wherever a repeat must not double-apply.
- Safe migrations (consistent with the project's Footgun Register; reversible where possible).
- Unit tests for the critical logic.

**Definition of Done:** AC covered; error codes match the contract; sensitive operations idempotent; migrations reversible or safe; tests green.

**Anti-patterns:** the wrong error code (for example an authorization error for domain validation); missing idempotency on any retry-prone operation (external webhooks, payment callbacks, and similar are common examples, but the rule is general); a migration that ignores footguns; logic with no tests.

**Top levers:** the error-code contract, because a misclassified error can silently log users out or hide a real failure; idempotency, because at-least-once delivery is the default in real systems; safe migrations, because a bad one is the hardest failure to undo in production.

## Frontend

**Mission:** Build the UI that realizes the flow from the AC, completable on every target device, consistent with the design system and the project's UX patterns, handling edge states (loading / empty / error / offline).

**Output spec:**
- Components per the flow, traced to the AC IDs.
- Device coverage: every flow completable on each target form factor (the specific target, for example the smallest supported screen, is injected from PROJECT_PROFILE.md).
- Edge states: loading, empty, error, offline.
- Consistency with the project's UI primitives and design system.
- Chosen UX patterns cited with a rationale (cite the pattern you chose and why; do not prescribe a specific pattern library here, that choice lives in PROJECT_PROFILE.md).
- Accessibility to the project's threshold.

**Definition of Done:** flow completable on the target devices; edge states handled; consistent with the design system; user-facing copy in plain language (no jargon, no technical IDs).

**Anti-patterns:** a desktop-only wall (a flow that cannot be completed on a smaller target); missing empty or error states; native primitives instead of the design system; jargon in copy.

**Top levers:** device coverage, because a flow that breaks on a target form factor is effectively unshipped for those users; edge states, because empty and error are where real users land first; design-system consistency, because it is what keeps the product coherent as it grows.

## QA

**Mission:** Prove binarily that the AC are met, on every target device, including edge states and error paths. QA maps tests 1:1 onto AC.

**Output spec:**
- A test -> AC matrix (every AC ID has at least one test).
- Cases: happy plus edge plus error.
- A device-coverage test before release (the target devices come from PROJECT_PROFILE.md).
- Test data per relevant variant axis (for example role, plan, locale, whatever variants the product actually has).
- A report of what passed and what did not.

**Definition of Done:** every AC has a test case; device coverage checked; error paths verified (correct codes); no open blockers.

**Anti-patterns:** a happy-path-only test; skipped device coverage; no mapping to AC; "works on my machine" instead of a matrix.

**Top levers:** the test -> AC mapping, because it turns "we tested it" into provable coverage; the device-coverage gate, because cross-device breakage is the most common late surprise; error paths, because they are where incorrect codes and silent failures hide.

## Security

**Mission:** Verify authorization, data protection (especially sensitive or personal data), data residency where it applies, and abuse surfaces. Security is the last gate before releasing risky surfaces.

**Output spec:**
- Authorization control per endpoint (who can do what).
- Data classification plus protection: classify each data flow by sensitivity level, then apply protection to the level (encryption, anonymization, residency) where it applies.
- Abuse surfaces: rate limiting, fraud, enumeration.
- Error-code consistency (never leak authorization as validation or the reverse).
- Compliance with the applicable regulatory regime (name the regime in PROJECT_PROFILE.md; the playbook stays neutral on which one applies).

**Definition of Done:** authorization verified; sensitive data protected per its class; abuse surfaces covered; no privilege-escalation path.

**Anti-patterns:** authorization that "trusts the frontend"; sending sensitive data to external services with no basis; no rate limit on public surfaces.

**Top levers:** authorization per endpoint, because a single missing check is a full breach; data classification, because protection only makes sense once you know what each flow carries; abuse surfaces, because public endpoints are probed automatically from day one.

## Post-release verification (closing the loop)

The success metric and counter-metric defined by BA are not closed by QA (which only proves AC are met pre-release). One role must own post-release verification: observability and logging in place, error monitoring live, a rollback trigger defined, and a scheduled check of whether the success metric actually moved and the counter-metric did not degrade. Assign this ownership explicitly (often Tech Lead or Backend). Without it, the loop BA opened is never closed and the feature's value is unproven. This maps to the VERIFY status in STATUS_TAXONOMY.md.

---

# Gates

- **Gate 0 (Recon):** SA has proven what already exists; zero designing from scratch for things already shipped.
- **Gate per role (Definition of Done):** the role's output is complete before it passes on.
- **Gate Release (pre-flight):** all of the project's release conditions are checked off (see PROJECT_PROFILE.md for the concrete list), only committed work is released, plus verification on a transitional environment (staging) if one exists. This corresponds to entering STAGING then SHIPPED in STATUS_TAXONOMY.md.
- **Feature-level Definition of Done (release acceptance):** the feature is not done until post-release verification confirms the BA success metric was checked and the counter-metric did not degrade (see Post-release verification above).

# Parallel tracks plus critical path

Lay out which roles and steps can run in parallel and which sit on the critical path. Give a single number of days for the critical path and a "blocks release: yes/no" flag per track. This realizes "parallel instead of serial" and gives the decision-maker the most important number.

---

# Adaptation to your project

The playbook is generic. To use it, inject your project specifics WITHOUT editing the core:
- **Project hard rules:** the list of rules BA gates at entry and SA confirms (for example API conventions, device-coverage targets, privacy rules, acceptance of AI artifacts, UX pattern library, undo-vs-confirm preferences). This is your "guardrail check".
- **Footgun Register:** the list of runtime traps in your stack, growing after every incident. This feeds the SA "footgun guard".
- **Release pre-flight:** the concrete deploy steps of your project. These feed Gate Release. Common examples to enumerate here: required environment variables set before deploy, a version bump on every deploy, lockfile sync with dependencies, persistence-layer entity and migration registration, and any other condition that, if missed, takes the release down.
- **Design system / UX patterns / accessibility threshold:** these feed the Frontend and QA roles, including the specific target form factors (for example the smallest supported screen) and the chosen UX pattern library.
- **Data classification levels and regulatory regime:** the sensitivity levels and the applicable legal framework that feed the Security role.
- **Personas and metrics:** from your Discovery or Brief (see DISCOVERY_PLAYBOOK.md), feeding BA.
- **Estimation unit:** fix the unit (ideal days, story points, or t-shirt sizes) so estimates are comparable across features.

Keep these things in a separate project file (for example PROJECT_PROFILE.md) and leave this playbook clean and shared across projects.
