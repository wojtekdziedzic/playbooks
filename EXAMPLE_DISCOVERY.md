# Worked Example: a full Discovery Sequence run (fictional)

This is a TEACHING EXAMPLE. Every number, persona, quote, and competitor below is INVENTED. It shows how to run the Discovery Sequence end to end on a made-up product, so you can see what each of the 8 roles produces, how the gates resolve, and how a Go/No-Go verdict is reached. It is NOT a real product, NOT real data, and has no connection to any actual project. Use it alongside DISCOVERY_PLAYBOOK.md (the method) and STATUS_TAXONOMY.md (the statuses).

The example is deliberately instructive: it does NOT end in a clean Go. It ends in a WEAK verdict with a designed fake-door RAT and an explicit persevere / pivot / kill rule, because that path teaches the method better than a rubber-stamp Go. The filled-in Brief template is shown at the end as the artifact that WOULD be produced if the RAT passes.

---

## The fictional setup

Product: "DeskHive", a SaaS for small and mid-size coworking spaces to manage hot-desk bookings (members reserve a desk for a day or a few hours from their phone).

The idea under Discovery: "Smart Desk Swap": when a member who booked a desk does not check in within 20 minutes of their slot start, DeskHive auto-releases that desk back into the available pool and (optionally) notifies waitlisted members that a desk just opened up.

Fictional project profile used for this run (the kind of thing that would live in PROJECT_PROFILE.md):
- North star: paid seats retained (monthly active paying spaces that renew).
- Outcome levers in play: retention (space operators renew), activation (members actually book).
- Signal sources: in-app booking events, no-show telemetry, operator support chats, 3 operator interviews.
- Persona segments: "Operator" (runs the space, our paying customer) and "Member" (books desks, not our customer directly).
- Cheap demand test of choice: fake-door tile inside the operator dashboard.

---

## PDL (Product Discovery Lead): orchestration

Sets the frame so the track stays cheap and converges to a binary decision.

- Opportunity Solution Tree:
  - Outcome (top): retention of paying spaces.
  - Opportunity: operators lose trust in DeskHive when paid desks sit empty while members are turned away ("ghost desks").
  - Idea: Smart Desk Swap (auto-release no-shows + notify waitlist).
  - Experiment (leaf): fake-door tile measuring operator intent to enable it.
- Exactly one riskiest assumption (named, owned by PDL): "Operators will TURN ON automatic desk release, accepting that DeskHive cancels a member's paid booking on their behalf." Everything else (notification copy, the 20-minute window, waitlist mechanics) is secondary.
- Time-box: one Discovery cycle = 5 working days. RAT, if triggered, max 10 days live.
- Loop control: max 2 iterations on this idea before escalation to a hard decision (kill, park with condition, or accept a named residual risk).
- Gate states at kickoff: all OPEN.

---

## PS (Product Strategist): problem and outcome

Frames the pain, not the feature, and attaches it to a lever.

- Problem statement (pain): "Operators see desks they sold sitting empty during peak hours because no-show members never release their booking, so the space looks full in the app but has open seats on the floor, and walk-in members get turned away."
- Outcome link (mechanism): retention. Empty-but-booked desks make the operator doubt the tool's accuracy; operators who stop trusting occupancy data churn at renewal. Reclaiming ghost desks raises usable capacity and renews trust.
- Current workaround: operators manually walk the floor, spot empty desks, and free them by hand in the admin panel (one fictional operator says she does this "three or four times a busy morning").
- Frequency and severity: peak mornings, 2 to 5 ghost desks per busy day per mid-size space; severity medium-high (lost walk-in revenue + eroded trust), but a manual workaround exists, which signals the pain is real yet partly absorbed.
- Anti-goal: this does NOT solve overbooking, pricing, or long-term desk assignment. It only reclaims no-show slots.

Self-attack: a manual workaround that "only takes a minute" is a yellow flag for willingness to adopt automation. Flagged for PRC and SKEPTIC.

---

## UXR (User Researcher): JTBD and signals

Separates what operators say from what they do.

- Persona: "Operator" (manages a 60 to 120 desk space, non-technical, lives in the dashboard during morning rush).
- JTBD: "When my space looks full in the app but I can see empty desks on the floor, I want the system to free up the no-shows automatically, so that I can seat walk-ins without policing every desk myself."
- Forces diagram:
  - Push (away from today): manual floor-walking is tiring and error-prone during rush.
  - Pull (toward Smart Desk Swap): automatic reclaim, less babysitting.
  - Habit: operators already trust their own eyes and the manual free-up button; it works.
  - Anxiety: "what if it cancels a member who was just parking the car?" Fear of angering a paying member.
  - Judgment: pull + push are real, but anxiety is high and habit is entrenched. Net force is NOT clearly positive. This is the crux.
- Evidence table:
  | Signal | Source | Strength | Suggests |
  |---|---|---|---|
  | No-show telemetry: ~7% of booked desks never check in | in-app booking events (behavioral) | strong | the ghost-desk problem is real and measurable |
  | "I walk the floor to free desks" | 2 of 3 operator interviews (declarative) | medium | manual workaround exists and is used |
  | "I would never let software cancel a member without me" | 1 of 3 interviews (declarative) | weak-but-loud | adoption anxiety around auto-cancel |
- Says vs does: operators SAY the empty-desk problem hurts (consistent), but they DO NOT yet do anything that proves they would hand the cancel decision to software (the behavioral gap).
- Sample sanity: 3 interviews, saturation NOT reached (third interview still surfaced a new objection). A base of 3 is a hypothesis, not evidence, for the adoption question. The no-show RATE is solid behavioral data; the WILLINGNESS-TO-AUTOMATE is not.

Self-attack: the strong signal (7% no-shows) answers "is there a problem", not "will they automate it". Those are different questions; do not let one launder the other.

---

## MKT (Market and Competition Analyst)

Situates the idea against alternatives and the moat.

- Landscape:
  - Direct competitors (fictional): "DeskFlow" and "SpotBee" both offer hot-desk booking; neither auto-releases no-shows today.
  - Real workaround (the true competitor): the operator's own eyes + the manual free-up button. Free, trusted, zero risk of angering a member.
- The gap: no incumbent automates no-show reclaim; the pain is acknowledged by operators but unserved by software.
- Moat-fit: reinforces the core (occupancy accuracy is DeskHive's selling point), not mere parity. Good fit.
- Time window: now-ish, not urgent. No competitor is shipping this; we are not racing anyone. The window is "soon" because occupancy trust is our differentiator, but there is no first-mover gun to our head.
- Competitive classification (market-position lens): differentiator (no one else has it).

Self-attack: "no one else does it" can mean untapped gap OR that operators do not actually want software making the cancel call. MKT cannot tell these apart; PRC and EXP must.

---

## PRC (Pricing and Behavioral Analyst): willingness plus Kano

Answers "is it worth it" and whether behavior will actually change.

- Kano classification: performance for the OPERATOR (more reclaimed desks = more satisfaction, scales with how well it works). Risks being indifferent or even a reverse feature for the MEMBER (a member whose booking gets auto-cancelled is unhappy). Two-sided, and the member side can backfire.
- Willingness signal: none yet behaviorally. Operators do not pay extra for the manual button today, and the workaround is "free". No price-based willingness evidence exists. To be validated in EXP.
- Value vs effort (single idea, simple scoring): value medium-high (touches the core trust lever); effort medium (no-show detection + auto-release + waitlist notify + an operator opt-in setting + member-facing cancel notice).
- Counter-cost (the tail, not just the build): support load from angry auto-cancelled members; an operator-configurable grace window to maintain; edge cases (member checks in at minute 21). This tail is non-trivial.
- Lightweight feasibility check: no obvious blocker. Check-in events and slot times already exist in the data model; auto-release is a scheduled job. Deep feasibility deferred to SA in Delivery.

Self-attack: classic delighter-with-no-must-have trap avoided (this is performance, tied to a real lever), but the member-side backfire and the absent willingness signal are unresolved. Do not let medium-high value paper over zero demand evidence for the ADOPTION behavior.

---

## DATA (Analytics): baseline, metric, instrumentation

Pins the numbers and pre-registers the RAT threshold.

- Baseline (from a real source, fictional here): no-show rate = 7.0% of booked desk-slots over the last 30 days (in-app booking events). Manual free-ups logged: ~38% of those no-shows are eventually freed by hand; the other ~62% sit empty until the slot ends.
- Success metric (binary checkable): of the ghost desks NOT freed manually today, reclaim at least 50% automatically within 6 weeks of enabling Smart Desk Swap, with no rise in member complaints. Baseline 0% auto-reclaim -> target 50% -> window 6 weeks.
- Counter-metric (what must not drop): member booking retention. If auto-cancel drives members to book less (or complain more), the feature fails even if it reclaims desks. Hard ceiling: member complaint rate must not rise above +1 per 100 bookings.
- RAT instrumentation: a fake-door tile in the operator dashboard. Measure: enable-intent clicks, and of those, completion of a confirmation step ("Yes, let DeskHive auto-cancel no-shows for my space").
- Pre-registered RAT success threshold (the contract handed to EXP): >= 30% of operators who SEE the tile click "Enable", AND >= 50% of clickers complete the confirmation step. Below that = the adoption assumption is not supported.
- Sample sanity: qualitative-to-behavioral hybrid. Defensible if the tile is shown to >= 40 distinct operator accounts over the test window (a stated minimum, not a p-value claim).

Self-attack: a click on "Enable" is intent, not the real behavior of living with auto-cancels for weeks. The threshold measures appetite, not durability; that limit is stated, not hidden.

---

## EXP (Experiment Designer): the RAT

Designs the cheapest test that can KILL the idea, using DATA's pre-registered threshold.

- Riskiest assumption (falsifiable, from PDL): "Operators will turn on automatic desk release, accepting that DeskHive cancels a paying member's booking on their behalf."
- Chosen method: fake-door / painted-door tile in the operator dashboard. Cheapest adequate method because the open question is INTENT TO ADOPT, which a tile measures with zero backend (no scheduler, no notifications, no member-facing flow built).
- Why not cheaper: a conversation already happened (3 interviews) and gave loud-but-thin signal; we need a behavioral click from many operators, which the tile provides. Why not more expensive: building the real auto-release first would be exactly the days-of-waste Discovery exists to prevent.
- Setup: one dashboard tile "Smart Desk Swap (new): auto-free no-show desks" with a short value line and an "Enable" button; clicking opens a confirmation modal; confirming shows an honest message: "Thanks. Smart Desk Swap is not live yet. We are gauging interest and will email you when it is ready." No setting is actually toggled.
- Per-test budget (agreed before start): max 10 days live, max 1 engineer-day to build the tile + modal + event logging. If it cannot be built in that budget, it is not a fake-door.
- Decision rule (written BEFORE the test, bound to DATA's threshold):
  - PERSEVERE (Go): >= 30% of operators who see the tile click Enable AND >= 50% of those confirm. Adoption appetite supported; proceed to Brief and Delivery.
  - PIVOT: enable-clicks clear 30% but confirmation craters (members-cancel anxiety kills it at the modal). Re-scope to "suggest a release, operator approves with one tap" (human-in-the-loop) and re-run.
  - KILL: < 15% click Enable. Operators do not want this; park or kill, do not re-argue.
  - (Between 15% and 30%, or the ambiguous middle: one more iteration only, per PDL's cap of 2.)
- What the test does NOT validate: durability of use over weeks, member-side complaint rate, the right grace-window length, notification copy.
- Honesty guardrail: the fake-door takes no money, toggles nothing, and after the click shows the clear "not live yet" message. Trust is preserved.

Self-attack: the pivot branch is the real value of this RAT; if confirmation collapses, that is a finding, not a failure.

---

## SKEPTIC (Red-team): adversary

Defaults to No-Go and forces the rest to disprove it.

- Attack on evidence: the only STRONG signal (7% no-shows) proves the problem, not the solution's adoption. The adoption signal rests on 3 interviews, one of which loudly objected. Sample too thin to claim demand for auto-cancel. Standing.
- Attack on demand: alternative explanation for "operators want it": they want EMPTY DESKS FREED, not necessarily SOFTWARE deciding to cancel. A one-tap "release this no-show?" prompt might satisfy the same job with none of the anxiety. The declared interest may be for the outcome, not this mechanism.
- Attack on value: counter-cost underweighted. Auto-cancelling a paying member who was 2 minutes late is a trust grenade; support load and member churn could exceed the reclaimed-desk gain.
- Attack on the moat: real, but "no competitor has it" might mean the market already learned members hate auto-cancel.
- Pre-mortem ("it shipped, it failed, why", 3 causes):
  1. Members revolt over surprise cancellations; operators disable it within a week.
  2. Operators never turn it on at all (anxiety > appetite); feature ships to zero adoption.
  3. The 20-minute window is wrong for most spaces and there is no good universal default; constant complaints.
- Decision-maker vs user: this looks like what a DASHBOARD-WATCHING OPERATOR wants (tidy occupancy), but the MEMBER who gets cancelled is a silent loser. Watch the two-sided risk.
- Verdict: WEAK. Not KILL (the problem is real and a cheap test can settle it), not SURVIVES (the adoption behavior is unproven and one interview actively pushed back). Exact evidence that would flip WEAK to SURVIVES: the fake-door RAT clearing DATA's pre-registered threshold (>= 30% enable-clicks AND >= 50% confirmation), OR a behavioral signal that operators already opt into auto-cancel-like automation elsewhere.

---

## Gate states and Go/No-Go verdict

| Gate | State | Why |
|---|---|---|
| Gate 0: Outcome fit | PASS | Attaches to retention with an explicit mechanism (occupancy trust drives renewal). |
| Gate 1: Evidence | PASS | 7% no-show rate is a strong behavioral signal that the ghost-desk problem is real. |
| Gate 2: Demand | OPEN | Demand for THIS mechanism (auto-cancel) is unproven; a fake-door RAT is designed to measure it. |
| Gate 3: Worth-it | OPEN | Value medium-high, but counter-cost (member backfire) and willingness are unresolved until the RAT. |
| SKEPTIC | WEAK | Problem real, adoption behavior unproven; named the exact evidence to flip it. |

VERDICT: NEED-MORE (WEAK). Not a Go, not a No-Go. The idea moves to status RAT per STATUS_TAXONOMY.md, carrying the named riskiest assumption and the designed fake-door test. The Demand and Worth-it gates stay OPEN until the RAT returns a number against DATA's pre-registered threshold.

What happens next, by branch (decided in advance, not after the result):
- RAT passes the threshold -> the WEAK flips to SURVIVES, all gates PASS, verdict becomes Go, the Brief below is filled and the idea moves to READY.
- RAT clears enable-clicks but fails confirmation -> PIVOT to the human-in-the-loop "approve release" variant, re-run once (within PDL's 2-iteration cap).
- RAT < 15% enable-clicks -> KILL (does not return without a new signal) or PARK with the trigger "revisit if no-show rate exceeds 12% or 3+ operators request auto-release unprompted".

---

## Brief to Delivery (shown filled, conditional on the RAT passing)

This is the artifact that the Go branch WOULD produce. Numbers shown are the illustrative passing result; they are fictional.

```
## Discovery to Delivery Brief: Smart Desk Swap
Date: 2026-06-23   Verdict: GO

PROBLEM (user pain, not a feature): Operators lose walk-in revenue and trust because no-show members never release their booked desks, so the app shows full while desks sit empty.
PERSONA + JTBD: Operator, "When my space looks full but I see empty desks on the floor, I want no-shows freed automatically, so that I can seat walk-ins without policing every desk."
OUTCOME (lever): retention. Reclaiming ghost desks restores occupancy-data trust, which drives renewal of paying spaces.
DEMAND EVIDENCE: fake-door RAT: 41% of operators who saw the tile clicked Enable; 63% of those confirmed auto-cancel (both above the pre-registered 30% / 50% threshold).
SUCCESS METRIC: auto-reclaim of non-manually-freed ghost desks 0% -> 50% within 6 weeks of enabling.  |  COUNTER-METRIC: member complaint rate must not rise above +1 per 100 bookings.
VALUE vs COST: Kano=performance (operator side)  score=medium-high  effort~medium (detection + auto-release job + waitlist notify + opt-in setting + member cancel notice)
MOAT-FIT: core (occupancy-trust differentiator)   WINDOW: now (no competitor has it; reinforces our selling point)
RISKIEST ASSUMPTION (validated): "operators will enable software-driven auto-cancel of paying members": confirmed by the fake-door clearing both pre-registered thresholds.
BOUNDARIES / OUT OF SCOPE v1: no overbooking logic, no pricing, no long-term desk assignment; grace window fixed at 20 min for v1 (configurable later).
GUARDRAILS FOR BUILD (already known): member-facing cancellation must be honest and reversible-within-grace; never cancel without a member notification; opt-in per space, default OFF (source: PROJECT_PROFILE.md two-sided-trust rule).
SKEPTIC VERDICT: SURVIVES, the fake-door cleared both pre-registered thresholds and the pivot branch was not needed; member-side counter-metric is carried into Delivery as a hard guardrail.
```

---

## What this example is meant to teach

1. The strong signal (7% no-shows) proves the PROBLEM; it does not prove ADOPTION of the chosen mechanism. Discovery keeps those two questions separate.
2. A loud single-interview objection is enough to keep the Demand gate OPEN, not enough to KILL. The right move is a cheap test, not an argument.
3. DATA pre-registers the threshold; EXP consumes it unchanged; the decision rule (persevere / pivot / kill) is written BEFORE the test. No moving the goalposts after the result.
4. A WEAK verdict is a productive outcome: it names the exact evidence that flips it, routes the idea to RAT (not to a vague backlog), and sets a 2-iteration cap so it cannot churn forever.
5. The Brief is only filled on a Go, and it carries the counter-metric and project guardrails forward so Delivery inherits the constraints, not just the feature.
