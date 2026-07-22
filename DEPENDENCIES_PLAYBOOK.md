# Dependencies Playbook: cross-team coordination

Generic and project-agnostic. Use it whenever a build needs something the team does not fully control: another team's API, a platform change, an external vendor, a sign-off, shared infrastructure. Inject the project specifics (team names, escalation contacts, SLAs) in the "Adaptation to your project" section and in `PROJECT_PROFILE.md`, never into the core.

Why: single-team delivery is covered by `DELIVERY_PLAYBOOK.md`, but plans usually die at the seams BETWEEN teams: the dependency nobody wrote down, the contract agreed verbally, the "they know we need it" that turns out false at integration time. `STATUS_TAXONOMY.md` gives blocked work a status (BLOCKED, with a named unblocker) but no operating procedure. This file is that procedure.

Relation to the other files: `DELIVERY_PLAYBOOK.md` (Tech Lead's integration contracts and critical path are where dependencies surface), `STATUS_TAXONOMY.md` (BLOCKED entry and exit rules), `DISCOVERY_PLAYBOOK.md` (a dependency that makes an idea infeasible is a PRC feasibility flag before it is a Delivery problem).

---

## 1. The Dependency Register

One table per feature (a section in the living spec, next to the Tech Lead plan), one row per dependency. A dependency that is not a row does not exist as far as planning is concerned.

| Field | Meaning |
|---|---|
| ID | DEP-n, referenced from plan steps ("step 4 depends on DEP-2") |
| What | the concrete thing needed (an endpoint, a schema change, a credential, a sign-off) |
| Direction | we-need-them / they-need-us (register both: being someone else's blocker is also a dependency) |
| Owner (ours) | the named person on our side who drives it |
| Owner (theirs) | the named person on the other side who committed (a team name is not an owner) |
| Type | blocking / soft / informational (see section 2) |
| Need-by | the date it must land, derived from the critical path (see section 4) |
| Status | requested / committed / delivered / verified (see section 5) |
| Fallback | what we do if it slips (mandatory for blocking, see section 6) |

## 2. Dependency types

- **Blocking**: sits on the critical path; if it slips, the release slips. Gets a fallback, a need-by date, and escalation rights.
- **Soft**: needed eventually but parallelizable; work proceeds against a stub or contract and integrates later. A soft dependency with a passed need-by date becomes blocking: re-triage it.
- **Informational**: no artifact needed, only awareness (another team must know we are changing something they read). Cheap to satisfy, expensive to skip: record it and send the notice.

## 3. Contract first, integration second

For any dependency that crosses a team boundary, the interface is agreed in WRITING before either side builds: endpoint shape, event schema, shared types, error codes, and who owns versioning. This extends the Tech Lead's "integration contracts up front" rule in `DELIVERY_PLAYBOOK.md` across the team boundary. The contract is the consumer's need stated first (consumer-driven), reviewed by the provider, and stored where both teams can see it. A verbal agreement is a risk, not a plan: if it is not written, the register row stays in "requested".

## 4. Timing: declare early, date backward

- Dependencies are surfaced at PLANNING time, not integration time: SA's recon marks external surfaces, and the Tech Lead's step plan turns each into a register row before the build starts. A dependency discovered during integration is a process failure worth a retro note.
- **Need-by is computed, not wished**: take the critical-path date of the first step that consumes the dependency and subtract an integration buffer (default: 20 percent of the remaining critical path, minimum 1 day). That is the need-by date on the row, and it is the date the other team commits to, not our internal deadline.

## 5. Status handshake (per row)

```
requested  -> we asked, no commitment yet (work must not assume it)
committed  -> named owner on their side agreed to the need-by date
delivered  -> they say it is done
verified   -> WE confirmed it works against the contract (integration test,
              not their word). Only "verified" unblocks dependent steps.
```

The delivered -> verified gap is the cross-team version of the VERIFY recon in `STATUS_TAXONOMY.md`: reportedly done is not done.

## 6. The fallback rule

Every BLOCKING dependency names its fallback before the build starts, chosen from (in order of preference): build against a stub or mock and integrate later; hide behind a feature flag and ship without; descope the dependent slice to a later phase; slip the release (a legal fallback, but it must be CHOSEN by the decision-maker, not discovered). "We wait" is not a fallback, it is the absence of one. The fallback converts BLOCKED from a stall into a decision.

## 7. Escalation ladder

Escalation is a service to the plan, not an aggression. Default ladder and clocks (override in `PROJECT_PROFILE.md`):

1. **Peer level**: owner to owner. No response or no commitment within 2 working days -> up.
2. **Lead level**: Tech Lead / team lead to their counterpart, with the register row and the critical-path impact in one message. No resolution within 2 working days -> up.
3. **Decision-maker level**: the single accountable decision-maker chooses: re-prioritize the provider team, accept the fallback, or accept the slip. This is a decision with a criterion, per the decision rules in `DELIVERY_PLAYBOOK.md`.

The clock starts at the FIRST unanswered request, not at the need-by date: escalating on the deadline is escalating too late.

## 8. Mapping to the status taxonomy

- An item goes BLOCKED only when a blocking dependency has passed its need-by date AND the fallback is not viable. The register row IS the "who or what unblocks it" that `STATUS_TAXONOMY.md` requires.
- While a fallback keeps work moving, the item stays in DELIVERY (the dependency is managed, not blocking the status).
- BLOCKED items are reviewed at the same cadence as PARKED reviews; a BLOCKED item whose unblocker has no committed date after escalation level 3 is re-decided (descope, fallback, or park with a trigger), not left to age.

## 9. Anti-patterns

- The undocumented dependency: "they know we need it" (no row, no owner, no date).
- A team name in the Owner (theirs) field instead of a person.
- A verbal contract, integrated against memory, diverging at the seam.
- Discovering a dependency at integration time and calling it bad luck.
- Escalating on the deadline day, or never, because "we do not want to bother them".
- BLOCKED as a comfortable parking lot: no unblocker date, no review, no fallback re-check.
- Being someone else's silent blocker: their dependency on us is not in our plan.

---

## Adaptation to your project

Keep the specifics in `PROJECT_PROFILE.md`, leave this core clean:

- **Team and contact map**: who owns which system, and the named escalation counterpart per team.
- **Escalation clocks**: override the 2-working-day defaults to match your org's tempo.
- **Contract storage**: name where cross-team contracts live (API spec repo, schema registry, shared doc) so "in writing" has one canonical place.
- **Integration buffer**: override the default need-by buffer (20 percent, minimum 1 day) to match how volatile your integrations are.
- **External vendors**: for third parties, add the contractual lever (SLA, support tier) to the register row, since the escalation ladder above stops at your org's boundary.
