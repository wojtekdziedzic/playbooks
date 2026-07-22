# PROJECT_PROFILE template: the project-specific sidecar

Copy this file into YOUR project as `PROJECT_PROFILE.md`, fill in every `<...>` placeholder, and keep the playbooks themselves clean and generic. This is the single canonical home for everything the playbooks call "project specifics": every field below exists because a named role, gate, or rule consumes it, and the "Consumed by" line under each field tells you which one. Fill it section by section as each track first needs it, or all at once; delete the consumers' notes once your team knows them by heart.

Defaults: where a playbook ships a default value (the PARKED cap, the fast path threshold, escalation clocks, the integration buffer), leaving the field at "default" means you accept the playbook's number. Override only with a reason, so the override is auditable.

---

## 1. Identity and outcome

- Product: `<name, plus one line on what it is>`
- Core value (the product's reason to exist): `<one sentence>`
  - Consumed by: Concept-First L5 (gaps are ranked by impact on core value), MKT (moat-fit).
- North star metric (exactly one; see METRICS_PLAYBOOK.md section 2 for the criteria): `<metric>`
- Lever decomposition (replace the example set: acquisition, activation, retention, revenue, compliance, debt): `<your levers>`
  - Consumed by: PS (outcome link), STATUS_TAXONOMY.md section 5 (the Outcome metadata every live item carries), the metric stack in METRICS_PLAYBOOK.md.
- Usage cycle length (the product's natural rhythm, used to size metric windows): `<for example daily use / weekly workflow / monthly cycle>`
  - Consumed by: METRICS_PLAYBOOK.md section 3 (a window is at least one full usage cycle).

## 2. Domain (Concept-First inputs)

- Domain nouns (the entities in the user's language, not the schema's): `<list>`
- Bounded contexts: `<name them ONLY where the same noun means materially different things in different parts of the business; otherwise "none">`
- Code locations for L6 reconciliation: entity directory `<path>`, persistence layer `<path>`, migration history `<path>`
- Code-reading fan-out available: `<yes / no>`
- Telemetry available as an evidence source: `<yes / no, and where>` (code shows what exists and what is read; usage frequency needs telemetry)
  - Consumed by: Concept-First L1 (naming), L6 (as-is reconciliation), and its reliability boundary.

## 3. Personas and signal sources (Discovery inputs)

- Persona segments: `<the real segments of your users, one line each>`
- Signal sources: `<where behavior and feedback come from: analytics, support, error logs, interviews, sales calls>`
- Cheap demand test of choice: `<fake-door / smoke test / landing-page signup / concierge MVP>`
- Discovery cadence defaults: cycle time-box `<n working days>`, RAT max time live `<n days>`, per-test budget `<max engineer-days and max cost>`, loop cap `<max iterations per idea before escalation>`
  - Consumed by: UXR (signals), EXP and DATA (test design and budget), PDL (time-box and loop control).

## 4. Metrics operating rules

- Sources of truth for baselines and checks: `<analytics stack, logs, tracker: name the systems>`
- Standard counter-metrics menu (recurring ones features pick from): `<for example error rate, support ticket volume, churn, task completion>`
- Post-release check ritual: `<which recurring meeting the metric check is folded into>`, default check owner: `<role>`
  - Consumed by: DATA (baselines), BA (success metric), post-release verification in DELIVERY_PLAYBOOK.md, METRICS_PLAYBOOK.md sections 3, 4, and 9.

## 5. Hard rules (the guardrail check)

The rules BA gates at entry and SA confirms, answered YES / NO / N.A. per feature. Keep each rule binary and name its source.

| # | Rule | Source / reason |
|---|------|-----------------|
| 1 | `<for example: no personal data leaves our infrastructure without a legal basis>` | `<law / policy>` |
| 2 | `<for example: every destructive action is undoable or double-confirmed>` | `<UX principle>` |
| 3 | `<for example: API responses follow the project error-code convention>` | `<engineering convention>` |

  - Consumed by: BA guardrail check, SA product gates, the "GUARDRAILS FOR BUILD" field of the Discovery Brief.

## 6. Footgun Register

Runtime traps in YOUR stack: things the compiler and types do not catch but that blow up in production. SA checks them off per feature (YES / NO / N.A. plus mitigation). The register grows after every incident: add a row, do not rely on memory.

| # | Footgun | When it bites | Mitigation | Added after |
|---|---------|---------------|------------|-------------|
| 1 | `<for example: lazy-load queries inside a loop>` | `<list rendering over a relation>` | `<eager-load or batch>` | `<incident or date>` |
| 2 | `<for example: a migration that locks a hot table>` | `<deploy under load>` | `<online migration pattern>` | `<incident or date>` |

  - Consumed by: SA footgun guard, Backend (safe migrations).

## 7. Delivery conventions

- Estimation unit: `<ideal engineer-days, or story points with a stated points-to-days mapping, or t-shirt sizes with stated ranges>`
- Design system / UI primitives: `<name>`
- UX pattern library: `<name>`
- Target form factors: `<for example: smallest supported screen width, tablet, desktop>`
- Accessibility threshold: `<for example: WCAG 2.1 AA>`
- Copy language and tone: `<language(s); plain language, no technical IDs in user-facing copy>`
  - Consumed by: Tech Lead (fixes the estimation unit), Frontend (design system, devices, accessibility), QA (device-coverage tests).

## 8. Security and compliance

- Data classification levels: `<for example: public / internal / confidential / personal-data, with the required protection per level>`
- Regulatory regime(s): `<for example: GDPR, plus any sector-specific rules>`
- Route E compliance triggers (tailor the "legal / compliance" list from the taxonomy's decision tree to the regimes that actually apply): `<which obligations auto-qualify an item as Route E, Q3a>`
  - Consumed by: Security role, Concept-First L3 (privacy/retention class and deletion owner), STATUS_TAXONOMY.md route tree Q3a.

## 9. Environments and release pre-flight

- Environment mapping: STAGING = `<your pre-production name>`, SHIPPED = `<your production name>`
- Release pre-flight checklist (every line binary; absent any of them: do not release):
  - [ ] `<required environment variables set before deploy>`
  - [ ] `<version bump on every deploy>`
  - [ ] `<lockfile in sync with dependencies>`
  - [ ] `<persistence-layer entities and migrations registered>`
  - [ ] `<add every other condition that, if missed, takes the release down>`
  - Consumed by: Tech Lead (the per-feature pre-flight subset), Gate Release in DELIVERY_PLAYBOOK.md, the STAGING to SHIPPED transition in STATUS_TAXONOMY.md.

## 10. Status operations

- Status source of truth: `<the ONE place a status lives: a tracker field, a backlog column, or a status line in the item's note>`
- PARKED cap: default 90 days | override: `<n days, plus the reason>`
- Fast path threshold: default 4 hours | override: `<n hours, plus the reason; keep it in hours, not days>`
  - Consumed by: the PO (owns transitions, STATUS_TAXONOMY.md section 7), the route decision tree Q4, the max-park-time rule.

## 11. Cross-team coordination (Dependencies inputs)

- Team and contact map:

| System / area | Owning team | Escalation counterpart (a person, not a team) |
|---------------|-------------|-----------------------------------------------|
| `<system>` | `<team>` | `<name>` |

- Escalation clocks: default 2 working days per rung | override: `<your clocks>`
- Contract storage: `<where cross-team contracts live: API spec repo, schema registry, shared doc space>`
- Integration buffer: default 20 percent of the remaining critical path, minimum 1 day | override: `<your buffer>`
- External vendors:

| Vendor | What we depend on | SLA / contractual lever |
|--------|-------------------|-------------------------|
| `<vendor>` | `<dependency>` | `<SLA tier, support contract>` |

  - Consumed by: the Dependency Register, the escalation ladder, and the need-by computation in DEPENDENCIES_PLAYBOOK.md.

---

## Maintenance

- Owner of this file: `<role or name>` (by default the PO owns sections 1 and 10; the Tech Lead owns 6, 7, and 9; Security owns 8).
- Review triggers: after every incident (section 6 grows), on strategy change (section 1), when a new team or vendor appears (section 11), and whenever a playbook default is overridden (record the reason next to the override).
