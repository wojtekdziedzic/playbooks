# Discovery Sequence Playbook: maximize every role

Generic, project-agnostic. Use it to run any should-we-and-what-to-build analysis before anything enters delivery. Inject the project specifics (outcome, deadline, signal sources, rules, personas) in the "Adaptation to your project" section, not in the core.

What it is: the UPSTREAM track that answers two binary questions before anything reaches Delivery: (1) is the problem real and what does the user actually expect, (2) is it worth building now. Discovery kills a weak idea with the CHEAPEST experiment (hours), before delivery spends days or weeks.

Relation to Delivery: Discovery ends BEFORE solution design begins. A Go output is a Brief that becomes the input to BA in the Delivery Sequence (see DELIVERY_PLAYBOOK.md). A No-Go output is a park with a revisit condition. Nothing enters Delivery without a Go.

Status vocabulary: this track produces and moves ideas across the statuses defined in STATUS_TAXONOMY.md (IDEA, DISCOVERY, RAT, READY, PARKED, KILLED). Use that file as the single source for status names and legal transitions.

Worked example: see EXAMPLE_DISCOVERY.md for a fully filled-in run of the sequence and the resulting Brief.

---

# Sequence: map

Roles: 7 sequential roles plus PDL as the continuous orchestrator = 8 total.

Orchestration throughout: PDL (Product Discovery Lead).

1. PS (Product Strategist): problem plus outcome (what it attaches to).
2. UXR (User Researcher): JTBD plus signals (what the user does, not says).
3. MKT (Market and Competition Analyst): who already does this, what is missing, what is the window.
4. PRC (Pricing and Behavioral Analyst): will anyone pay or use it, Kano, willingness.
5. DATA (Analytics): baseline plus success metric plus test instrumentation, and the pre-registered RAT success threshold.
6. EXP (Experiment Designer): RAT, the cheapest test that CAN kill the idea, using DATA's pre-registered threshold.
7. SKEPTIC (Red-team): adversary, tries to kill the idea and every section above.

Gates (binary, blocking):
- Gate 0: Outcome fit. The idea attaches to one measurable outcome with an explicit mechanism of impact. Missing = park.
- Gate 1: Evidence. The problem has evidence (a signal or behavior), not just "I think so".
- Gate 2: Demand. There is a demand signal (behavioral beats declared), or a designed RAT that will measure it.
- Gate 3: Worth-it. Value vs effort is resolved, the cost is worth the slot.
- Gate Go/No-Go: a binary decision. Go produces a Brief to Delivery. No-Go produces a park with a revisit condition.

Recursion: the track is not one-shot. It is a loop (see "Continuous loop"). An idea can pass through repeatedly until some gate yields a hard Go or a durable No-Go.

---

# Roles

## PDL (Product Discovery Lead): orchestration

Mission: keep the track CHEAP and CONVERGENT: one outcome, one riskiest assumption, a clear path to a binary decision. Owner of the Opportunity Solution Tree and the Go/No-Go gate. Protects the team's time: an hour of discovery must be cheaper than an hour of wasted build.

Output spec: Opportunity Solution Tree (outcome to opportunities to ideas to experiments); exactly one named riskiest assumption; a cycle time-box; gate states (PASS / FAIL / OPEN with reason).

Definition of Done: OST with the outcome at the top and no orphaned ideas; exactly one riskiest assumption; time-box set; Go/No-Go is binary with a named owner; loop-control rule set (max iterations before escalation, see "Continuous loop").

Anti-patterns: analysis paralysis; an idea with no outcome; five co-equal "risks" instead of one; designing the solution (that is Delivery's job); letting an idea churn through the loop with no iteration cap.

Top levers: one outcome plus one riskiest assumption; a time-box that forces a decision; Go/No-Go as the only exit into Delivery.

## PS (Product Strategist): Problem and Outcome

Mission: turn the idea into a sharply defined PROBLEM (not a solution) attached to a measurable outcome. Guards against designing a solution before the problem is named.

Output spec: problem statement (user pain, not a feature); outcome link (explicit mechanism of impact: acquisition / activation / retention / revenue / compliance); current workaround (what the user does instead today); frequency and severity; anti-goal (what the idea does NOT solve).

Definition of Done: problem framed as pain, not a solution; explicit mechanism of impact on the outcome; a real workaround described; severity estimated.

Anti-patterns: a problem disguised as a solution; an outcome like "the user will be happy" with no lever; a skipped workaround (a cheap workaround signals low willingness to pay).

Top levers: problem as pain; explicit mechanism of impact; workaround as a proxy for willingness to pay.

## UXR (User Researcher): JTBD and signals

Mission: establish what the user REALLY expects, separating what they say from what they do. Formalizes the Job-To-Be-Done and digs into real signals rather than inventing them. With a small user base, aim for deep qualitative and behavioral signals, not surveys.

Output spec: a named persona; JTBD in the form "When [situation], I want [motivation], so that [outcome]"; a forces diagram (push / pull / habit / anxiety, with a judgment of whether pull + push > habit + anxiety); an evidence table (signal / source / strength / what it suggests, with at least 1 non-declarative signal, or an explicit "no evidence = hypothesis for RAT"); a clear "says vs does" split. Qualitative sample sanity: state how many sessions or interviews and whether saturation was reached (new sessions stop surfacing new themes); a base of 1 or 2 is a hypothesis, not evidence.

Definition of Done: persona plus JTBD derived from a signal, not from assumption; forces filled in; at least 1 behavioral signal or an explicit hypothesis for RAT; the says/does gap checked; qualitative sample sized and its sufficiency stated.

Anti-patterns: a survey of a handful of users treated as data; a declaration of "I would use it" treated as demand evidence; armchair JTBD; leading questions; claiming a theme from a single interview.

Top levers: behavior beats declaration; the forces diagram (will the user change a habit); JTBD from a signal.

## MKT (Market and Competition Analyst)

Mission: situate the idea in the market: who already does this (products plus workarounds), what is missing, what is our window. Guards against chasing someone else's parity where we cannot win, and steers toward reinforcing a real moat.

Output spec: landscape (products plus real workarounds); the gap (what is missing or what users hate); moat-fit (does it reinforce the core or just parity); time window (now vs later, plus the reason); a competitive classification (must-have parity / performance / differentiator / nice-to-have).

Note on classification: MKT's classification is a market-position lens (where this sits versus competitors). PRC's Kano labels are a user-satisfaction lens (how users react to presence or absence). They are two distinct axes, not the same scale; an idea can be a market differentiator (MKT) yet a Kano delighter or even indifferent (PRC). Read them together, do not collapse one into the other.

Definition of Done: landscape including workarounds; a named gap with evidence; a moat-fit judgment; the window resolved.

Anti-patterns: building parity on the incumbent's strong field; ignoring the workaround; "let us build it because they have it" with no moat-fit.

Top levers: the workaround as the real competitor; moat-fit; the time window.

## PRC (Pricing and Behavioral Analyst): willingness plus Kano

Mission: answer "is it WORTH it": will anyone pay, will behavior actually change, and what is the effort relative to the return. Classifies with Kano and weighs value vs effort. Cuts ideas that are pleasant but move neither the wallet nor behavior.

Output spec: a Kano classification (must-have / performance / delighter / indifferent) with a reason; a willingness signal (or "none, to validate in EXP"); value vs effort (simple scoring, or RICE when ranking across multiple ideas); counter-cost (maintenance, support, complexity, not just the build).

On effort and feasibility: PRC estimates effort from a pricing and behavioral angle, not deep technical feasibility. Run a lightweight feasibility check here (is there an obvious blocker that makes this impractical or far more expensive than it looks). Defer deep technical-feasibility analysis to SA in Delivery; do not let a Go land on an idea with an obvious feasibility wall.

RICE, when used: Reach (how many are affected per period), Impact (how much it moves the outcome per case), Confidence (how sure the inputs are), Effort (person-time). Score = Reach x Impact x Confidence / Effort. Use simple value-vs-effort scoring for a single idea; reach for RICE only when ranking several ideas against each other.

Definition of Done: a Kano label; a willingness signal or an explicit hypothesis; value vs effort computed; the maintenance counter-cost computed; lightweight feasibility check done or deep feasibility explicitly deferred to SA.

Anti-patterns: a delighter when no must-have exists; "everyone will love it" with no willingness signal; effort counted as build only, with no maintenance tail; gold-plating.

Top levers: Kano (ordering); willingness beats enthusiasm; the maintenance counter-cost.

## DATA (Analytics): baseline plus metric plus instrumentation

Mission: pin down the numbers: a baseline from the real state, a success metric with target and window, a counter-metric, and how to instrument the RAT. Without DATA, discovery ends in "it probably works".

Output spec: baseline (a number from a real source, not from memory); success metric (baseline to target to window, binary checkable); a counter-metric; RAT instrumentation (how we will measure, with a success threshold set BEFORE the test); sample sanity (statistical vs qualitative, named explicitly; for qualitative, name what makes the sample defensible, for example saturation reached or a stated minimum number of sessions).

Handoff to EXP: DATA pre-registers the RAT success threshold (the number that flips persevere / pivot / kill). EXP consumes exactly this threshold in its decision rule. This is a contract: EXP must not invent or move the threshold after the fact.

Definition of Done: baseline from a real source; metric binary checkable; a counter-metric exists; the RAT has a measurement method and a threshold set before start; sample sanity checked and, if qualitative, its sufficiency stated.

Anti-patterns: a guessed baseline; a metric with no target or window; success only, with no counter-metric; faking statistics on a tiny sample; defining the measurement after the test.

Top levers: a baseline from data; a counter-metric; a success threshold set before the test.

## EXP (Experiment Designer): RAT (Riskiest Assumption Test)

Mission: design the CHEAPEST experiment that can KILL the idea before a single line of production code exists. Takes the riskiest assumption, picks the cheapest method from the ladder, and defines persevere / pivot / kill up front using DATA's pre-registered threshold.

Cheap-test ladder (cheapest first): a user conversation; fake-door / painted door (a tile or CTA that measures intent, zero backend); a clickable mockup; a landing or pricing page (when the question is willingness to pay); concierge / Wizard of Oz (do it by hand before automating).

Output spec: the riskiest assumption (falsifiable); the chosen method (and why it is the cheapest adequate one); setup (what we stand up, time and cost); a per-test budget (max time and max cost agreed before start, so a "cheap" test cannot quietly become a build); a decision rule set BEFORE the test (persevere / pivot / kill against DATA's threshold); what the test does NOT validate; an honesty guardrail (a fake-door takes no money and does not damage trust; after a click, show a clear message).

Definition of Done: the test targets the riskiest assumption; the cheapest adequate method; a per-test time and cost budget set before start; a decision rule written before start, bound to DATA's pre-registered threshold; the test's boundaries named; the honesty guardrail in place.

Anti-patterns: a test that cannot disprove the idea; validating an easy assumption instead of the riskiest one; building a "small MVP" when a fake-door would do; a decision rule written after the result; a fake-door that damages trust; a "cheap" test with no cost ceiling that balloons into a build.

Top levers: the fake-door (cheapest demand validator); a decision rule before the test; concierge before automation.

## SKEPTIC (Red-team): adversary

Mission: try to KILL the idea and undermine every section above. Defaults to "No-Go" and forces the rest to disprove it. Protects against falling in love with your own idea.

Output spec: attack on evidence (behavior or declaration, sample size, leading bias); attack on demand (an alternative explanation for the signal); attack on value (underestimated effort, counter-cost); attack on the moat; a pre-mortem ("it shipped, it failed, why", with 3 causes); an explicit "is this what a decision-maker wants or what the user wants"; a verdict of KILL / WEAK / SURVIVES with justification.

Verdict scale and re-entry: KILL = a fatal flaw with no cheap remedy, park or kill the idea. WEAK = a specific gap is unproven, name exactly what new evidence would flip it to SURVIVES (for example a passing RAT against DATA's threshold, or a behavioral signal UXR is missing). SURVIVES = every attack was answered with evidence. On re-entry through the loop, WEAK flips to SURVIVES only when that named evidence arrives, not by re-arguing.

Definition of Done: every section received a concrete attack; a pre-mortem with 3 causes; "decision-maker vs user" checked; an unambiguous verdict; for a WEAK verdict, the exact evidence that would flip it is named.

Anti-patterns: token skepticism; attacking only the weak sections; no pre-mortem; a WEAK verdict with no stated path to SURVIVES.

Top levers: a default "No-Go" to be disproven; the pre-mortem; "decision-maker wants vs user wants".

---

# Gate: Go/No-Go

Go only when ALL gates PASS and SKEPTIC = SURVIVES. Three exits:
1. Go: produces a Brief to Delivery (below), and the idea moves to READY per STATUS_TAXONOMY.md.
2. No-Go (park): with an explicit revisit condition ("we return when [condition]"). Without a condition the idea does NOT return (anti backlog bloat). This is the PARKED status in STATUS_TAXONOMY.md.
3. Need-more (WEAK): returns to EXP for a stronger RAT; the time-box and the PDL iteration cap keep this from becoming an endless loop.

# Template: Brief to Delivery

```
## Discovery to Delivery Brief: <name>
Date: <YYYY-MM-DD>   Verdict: GO

PROBLEM (user pain, not a feature): <1 sentence>
PERSONA + JTBD: <persona>, "When ..., I want ..., so that ..."
OUTCOME (lever): <acquisition / activation / retention / revenue + mechanism>
DEMAND EVIDENCE: <behavioral signal / RAT result with a number>
SUCCESS METRIC: <baseline -> target -> window>  |  COUNTER-METRIC: <what must not drop>
VALUE vs COST: Kano=<must-have / performance / delighter / indifferent>  score=<...>  effort~<...>
MOAT-FIT: <core / parity>   WINDOW: <now / later + reason>
RISKIEST ASSUMPTION (validated): <what it was, how it was disproven, vs DATA threshold>
BOUNDARIES / OUT OF SCOPE v1: <what is NOT included>
GUARDRAILS FOR BUILD (already known): <project rules that touch this feature; source them from the Adaptation to your project section / PROJECT_PROFILE.md>
SKEPTIC VERDICT: SURVIVES, <what was convincing>
```

# Continuous loop

Discovery is not a one-shot gate, it is a cycle:
1. Signals over a period (conversations, user behavior, errors, reports) feed an opportunity inbox.
2. Triage (PDL): which outcome does it serve, is it worth a cycle; larger ideas get a riskiest assumption.
3. Run 1 or 2 cheap RATs in parallel with ongoing build (two tracks, not a queue).
4. Go/No-Go: Go produces a Brief; No-Go parks with a revisit condition.
5. Periodic park review: condition met means back to the track, otherwise it stays dormant.

Recursive "max from each role": every role ends by self-attacking its own section; SKEPTIC attacks across all sections; the track loops until a gate yields a hard Go or a durable No-Go. Loop control: PDL sets a maximum number of iterations per idea (for example 3); on hitting the cap without a hard Go, escalate to a decision (kill, park with condition, or accept a named residual risk), rather than churning.

---

# Adaptation to your project

Inject the specifics (WITHOUT editing the core):
- Outcome / north star: one measurable goal that PS attaches problems to.
- Signal sources: where UXR draws behavior and feedback (analytics, conversations, support, errors).
- Project rules: what feeds the "guardrails for build" field in the Brief.
- Personas: the real segments of your users.

Keep these in a separate project file (e.g. PROJECT_PROFILE.md). Leave this playbook clean and shared across projects.
