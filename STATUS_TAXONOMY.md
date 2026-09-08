# Status Taxonomy: state of an idea across the tracks

Why: using "PARKED" as the single status for everything-not-shipped is a bottomless bag. It does not distinguish "not yet validated" from "validated but waiting for capacity" from "blocked by something external" from "rejected". The result: the backlog swells, and things that are actually done hang around mislabeled as "plan" (a common symptom: the status written in your notes drifts away from the real state of the product). This taxonomy gives every idea (a) a place in the flow of the tracks and (b) an explicit gate to the next step.

This file is the shared status vocabulary for the playbooks. The role tokens used below (PDL, BA, SA, Tech Lead, Backend, Frontend, QA, Security/Sec, PO) are defined in those files: see `DISCOVERY_PLAYBOOK.md` for the Discovery roles (PDL, PS, UXR, MKT, PRC, DATA, EXP, SKEPTIC) and gates (Outcome, Evidence, Demand, Worth-it, Go/No-Go), and `DELIVERY_PLAYBOOK.md` for the Delivery sequence (BA, SA, Tech Lead, Backend, Frontend, QA, Security). PO means the Product Owner who owns the backlog and the north-star metric for your project. A "slot" below means one unit of team delivery capacity: a build that the team can actually take on next.

Flow: `IDEA -> DISCOVERY -> (Go) -> READY -> DELIVERY -> STAGING -> SHIPPED`. Side states: PARKED, BLOCKED, KILLED. Control flags: PARTIAL, VERIFY. (RAT is a pipeline status inside Discovery, see section 1.)

---

## 1. Pipeline statuses (where in the flow)

| Status | Meaning | Entry condition |
|---|---|---|
| `IDEA` | raw idea, not triaged | landed in the opportunity inbox |
| `DISCOVERY` | in the Discovery track (the "should we / what" question) | PDL accepted it into the track; working through the Discovery gates (Outcome, Evidence, Demand, Worth-it) |
| `RAT` | Discovery surfaced a WEAK verdict (see verdicts below); waiting on a cheap experiment | named riskiest assumption plus a designed RAT (Riskiest Assumption Test) |
| `READY` | demand is settled (Go from Discovery OR a valid skip-Discovery reason); the Brief exists; waiting for a Delivery slot | Go/No-Go gate returned Go, or an explicit skip reason (see Route E) |
| `DELIVERY` | being built (the BA to Security sequence in `DELIVERY_PLAYBOOK.md`) | entered Delivery |
| `STAGING` | built, waiting on QA and promote | deployed to a pre-production / staging environment |
| `SHIPPED` | live in production, outcome confirmed | promoted to production and the target outcome was confirmed |

Discovery verdicts (the vocabulary used above and in the state machine): **Go** (demand is proven, proceed to READY), **No-Go** (do not build, route to PARKED or KILLED), **WEAK** (a key assumption is unproven, route to RAT for a cheap test before deciding). These three are the only legal Discovery outputs.

## 2. Control flags (still on the pipeline, but carrying a flag)

| Status | Meaning | Requires |
|---|---|---|
| `PARTIAL` | part is live in production, the rest is defined but not built | explicit split: what is done versus what remains (phase X done / phase Y open) |
| `VERIFY` | reportedly shipped or done, but unconfirmed (either the outcome or the bare fact of shipping) | a recon action: inspect the deployed source of truth (e.g. the released branch / the live build / the tracker) or measure the outcome metric |

## 3. Side states (off the flow, with a reason)

| Status | Meaning | REQUIRES (otherwise the status is illegal) |
|---|---|---|
| `PARKED` | deliberately deferred | a revival trigger plus a review date plus a max park time (see the cap rule below). Without a trigger it is not "parked", it is "lost" |
| `BLOCKED` | external blocker | who or what unblocks it (illustrative examples: legal sign-off, design input, third-party API credentials, a signed contract, a business decision) |
| `KILLED` | rejected or superseded | a reason. Does not return without a NEW signal |

**Max park time (the PARKED cap):** a park is not open-ended. Every PARKED item carries, besides the trigger and the review date, a maximum total park time: default 90 days, override per project in `PROJECT_PROFILE.md`. At each review date there are exactly three legal moves: (a) trigger met, the item returns to the flow (`DISCOVERY` or `READY`); (b) trigger not met but still plausible, the park is renewed with a new review date (the cap keeps counting total time parked); (c) neither, the item moves to `KILLED`. When total park time hits the cap, renewal is no longer the default: either a decision-maker explicitly re-justifies the park with a NEW named reason (which restarts the clock once), or the item is `KILLED`. A park with no cap regrows exactly the bottomless "PARKED" bag this taxonomy exists to prevent.

---

## 4. Route: which sequence (answering "both, or just one?")

**Intake (before the tree).** The route is itself a decision, so it needs input. A one-sentence request is not input. Before running the tree, fill six slots from what the requester already wrote: **Ask** (the deliverable in one sentence, plus its artifact type: plan / architecture / code / decision), **Job** (whose problem, when it occurs, what breaks without it), **Decision-maker** (who accepts the artifact, who can veto it), **Hard constraints** (stack, deadline, budget, legal or regulatory regime, organizational rules), **What already exists** (product, code, process, systems to integrate with), **Success signal** (how the requester will know it worked).

Intake rules:

- **Ask is the only blocking slot.** If the deliverable is unclear, ask and stop; do not run the tree. The other five never block.
- **Depth rule:** spend a question on a slot only when two plausible answers would change the artifact materially (route, scope, architecture). Name the two answers; if the artifact comes out identical under both, do not ask.
- **Recon before asking:** a slot answerable by reading the code, repo or docs is resolved by recon (Gate 0 in `DELIVERY_PLAYBOOK.md`), not by a question. This is what separates a codebase you can read from a client system you cannot.
- **One batch, at most five questions, then proceed.** Do not wait for the answers and do not ask in rounds. An intake that blocks costs more than the misroute it prevents.
- **Unfilled slots become numbered assumptions** at the top of the artifact: `A-n: <slot> = <default>; if false: <what changes>`. They are carried in the default-on-OK block (cross-cutting rule 1 in `DELIVERY_PLAYBOOK.md`). Defaults: Job = the ask taken literally with the requester as persona; Decision-maker = the requester; Hard constraints = the `PROJECT_PROFILE.md` rules and nothing beyond; What exists = whatever recon found; Success signal = "accepted by the decision-maker" (explicitly weak, flag it for replacement).
- **Intake scales to risk** like the tracks do: a trivial fast-path item (Q4 below) with inferable slots asks nothing at all.

| Route | Path | When |
|---|---|---|
| `D->E` | Discovery, then Delivery | unvalidated demand or monetization bet; competes for a slot |
| `E` | Delivery only (skip Discovery) | demand is not in question (see the rule below) |
| `D` | Discovery only (no build) | pure research or strategy |
| `--` | not an idea | reference / hard rule / strategy / ops |

**Route decision tree:** run top to bottom on every triaged item; the FIRST question answered YES assigns the route and the walk stops. No judgment calls outside the tree: if none of questions 1 to 4 fires, the answer is question 5.

```
Q1. Is it not an idea at all (reference, hard rule, strategy note, ops)?
      YES -> Route --
Q2. Is it pure research or strategy, with no build committed?
      YES -> Route D
Q3. Is demand certain BY DEFINITION? That means at least one of:
      a. legal / compliance obligation (privacy law such as GDPR, terms of
         service, accessibility, security, data residency)
      b. table-stakes parity: without it the product cannot compete at all
      c. tech-debt / infra / refactor (no question about user demand)
      d. finishing something already partly live in production (PARTIAL)
      e. a problem reported AND confirmed by the PO from observing a real user
      YES -> Route E (skip Discovery; record which letter applied)
Q4. Is it a trivial fix under the fast path threshold below?
      YES -> Route E (fast path)
Q5. Otherwise: a new user-facing feature with uncertain demand, a
    monetization bet, or a large idea competing for a limited team slot.
      -> Route D->E (Discovery required)
```

**Fast path threshold (Q4):** "trivial" is a number, not a feeling: estimated effort at most 4 hours end to end (build plus test plus release), AND no new persistent state, AND no new external dependency. All three must hold. If the estimate exceeds 4 hours, or the fix adds a new entity or a new integration, it is not trivial: go back to the tree and land on Q5. The 4 hour default lives here; override it per project in `PROJECT_PROFILE.md`, but keep it in hours, not days: a fast path measured in days is just an unreviewed build.

**Require Discovery (Route D->E, Q5):** Discovery kills a weak idea with a cheap demand test (for example a fake-door, a smoke test, or a landing-page signup) in hours, before Delivery spends days or weeks.

**Audit line:** every Route E item records which branch admitted it (Q3 letter a to e, or Q4 with the estimate), and every routed item records which intake slots were assumptions rather than facts. A Route E with no recorded reason is illegal, exactly like a PARKED with no trigger.

---

## 5. Required metadata (every live item)

- **Outcome**: which north-star lever it serves. The north star is your project's single most important success metric, declared in `PROJECT_PROFILE.md`. Example lever set (swap for your own): acquisition, activation, retention, revenue, compliance, debt. An idea without a lever has no right to take a slot.
- **Gate / Trigger**: what unblocks the next step, or the condition for return (for PARKED / BLOCKED).
- **Riskiest assumption**: the one assumption that, if false, kills the idea (mandatory for Route D->E; it is the entry condition into RAT).

---

## 6. Legal transitions (state machine)

```
IDEA -> DISCOVERY
DISCOVERY --WEAK--> RAT --> DISCOVERY            (loop: re-evaluate after the test)
RAT --assumption disproven--> PARKED | KILLED    (a failed RAT can end the idea)
DISCOVERY --Go--> READY                          (Route D->E)
DISCOVERY --No-Go--> PARKED | KILLED
(skip) -> READY                                  (Route E, demand certain)
READY -> DELIVERY -> STAGING -> SHIPPED
STAGING --QA fails--> DELIVERY                   (rework, then back to STAGING)
SHIPPED -> VERIFY                                (when the outcome or the fact is unconfirmed)
VERIFY --confirmed--> SHIPPED                    (recon proved it real, outcome OK)
VERIFY --not done / outcome failed--> DELIVERY   (re-open to build or fix)
DELIVERY | SHIPPED --scope split--> PARTIAL      (part live, rest defined)
PARTIAL --remaining scope picked up--> READY     (the rest re-enters via READY)
PARKED --trigger met--> DISCOVERY | READY
BLOCKED --unblocker removed--> previous status

Catch-all (overrides the linear chain): ANY state -> PARKED (with a trigger) | BLOCKED (with an unblocker) | KILLED (with a reason)
```

Hard rule: only `READY` enters `DELIVERY`. `READY` is produced either by a Go in Discovery or by an explicit skip under the Route E rule. Nothing enters Delivery sideways. The catch-all above takes precedence over the linear chain (any state can move to a side state), but it never creates a new sideways entry into DELIVERY: an item leaving BLOCKED returns to its previous status, and if that status was READY (or a re-opened PARTIAL routed through READY), entry into DELIVERY still goes through READY. So the "no sideways entry" rule and the catch-all do not conflict.

---

## 7. Ownership and recording

- The PO owns status transitions for an item and is accountable for keeping the recorded status honest.
- Record the status wherever your project tracks work (a tracker field, a backlog row, or a status line in the item's note). Pick one source of truth and keep it consistent: drift between the recorded status and the real product state is the exact failure this taxonomy exists to prevent. That is what the VERIFY recon action checks for.

---

## 8. Worked examples (generic)

1. **D->E feature, Go.** A new user-facing capability with uncertain demand starts at `IDEA`, is accepted into `DISCOVERY`, runs a cheap demand test, and the Go/No-Go gate returns Go. It becomes `READY` (Brief written), waits for a slot, enters `DELIVERY`, reaches `STAGING`, passes QA, and is promoted. Once the target outcome is observed it is `SHIPPED`.

2. **WEAK then killed via RAT.** An idea reaches `DISCOVERY`, the Demand gate is ambiguous, so the verdict is WEAK. The riskiest assumption is named and the item moves to `RAT`. The cheap test disproves the assumption, so the item goes to `KILLED` with a reason (it will not return without a new signal).

3. **PARKED with a trigger.** A large idea is real but not now. It moves to `PARKED` with a revival trigger ("revisit when [condition] is true") and a review date. When the trigger is met it returns to `DISCOVERY` (if demand is still open) or `READY` (if demand is already settled).

4. **VERIFY case.** An item is recorded as shipped, but no one confirmed the outcome. It is flagged `VERIFY`. The recon action inspects the live source of truth and measures the metric: if confirmed, it returns to `SHIPPED`; if it turns out it was never actually done, it is re-opened to `DELIVERY`.

---

## Adaptation to your project

This file is generic on purpose. To adapt it, keep all project specifics in a separate project file (for example `PROJECT_PROFILE.md`) and leave this taxonomy clean and shared across projects:

- **North-star levers**: replace the example lever set (acquisition, activation, retention, revenue, compliance, debt) with your own, and declare your single north-star metric in `PROJECT_PROFILE.md`.
- **Compliance triggers**: tailor the Route E "legal / compliance" examples to the regimes that actually apply to you (privacy law, accessibility level, security baseline, data residency, sector-specific rules).
- **Environments**: map `STAGING` and `SHIPPED` to your real environment names if they differ (pre-prod, canary, production, and so on).
- **Tracker mapping**: map each status onto your tracker (a label, a column, or a field) and record where the status lives (see section 7).
- **Cheap demand test**: pick the cheap test that fits your context (fake-door, smoke test, landing-page signup, concierge MVP) and name it in your profile.
- **PARKED cap**: override the default 90 day max park time if your planning cadence needs a different one, and record the override in `PROJECT_PROFILE.md`.
- **Fast path threshold**: override the default 4 hour trivial-fix ceiling in `PROJECT_PROFILE.md` if needed, keeping it in hours (see the rule in section 4).

For the gates that drive the DISCOVERY -> RAT -> READY transitions, see `DISCOVERY_PLAYBOOK.md`. For the build sequence behind `DELIVERY`, see `DELIVERY_PLAYBOOK.md`.
