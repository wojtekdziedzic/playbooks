# UI Playbook: evidence-graded interface decisions

Generic and project-agnostic. Use it whenever a UI decision is being made: a new screen, a redesign, a copy or layout change meant to move a metric, or a UI experiment. Inject the project specifics (design system, forbidden patterns, traffic per screen, testing tool) in the "Adaptation to your project" section and in `PROJECT_PROFILE.md`, never into the core.

Why: `DELIVERY_PLAYBOOK.md` asks Frontend to "cite the UX pattern you chose and why", and `DISCOVERY_PLAYBOOK.md` asks EXP to test the riskiest assumption, but neither says what counts as a good "why" for a UI choice. Without a standard, UI decisions are settled by taste, by the loudest person, or by copying a big company whose context is nothing like yours. This file is the shared standard: it separates hygiene (just do it) from levers (prove it), grades the evidence behind every lever, and puts persuasion patterns behind an ethics gate.

Relation to the other files: `DELIVERY_PLAYBOOK.md` (Frontend output spec and Definition of Done, QA device and state coverage), `DISCOVERY_PLAYBOOK.md` (EXP picks UI hypotheses from the register, PRC owns price framing, SKEPTIC runs the ethics gate), `METRICS_PLAYBOOK.md` (every lever pattern carries a success metric and a counter-metric, thresholds pre-registered), `STATUS_TAXONOMY.md` (a UI-only change is usually the Route E fast path, a conversion experiment is Route D or D->E).

---

## 1. Three kinds of UI decision

Classify every UI decision before arguing about it. The class decides how much evidence it needs.

| Class | What it is | Evidence needed | Example families |
|---|---|---|---|
| **Hygiene** | Baseline usability: the interface tells the truth about state and lets people recover from mistakes | None beyond the checklist in section 4. Do not A/B test hygiene; ship it | feedback after every action, empty / loading / error states, inline validation, distinct clickable vs selected styles, undo, labels on ambiguous icons, adequate hit areas |
| **Lever** | A change expected to move a named metric (conversion, activation, completion) | A register entry at E2 or better before a large build, or a pre-registered experiment (section 6) | fewer form fields, one-column layout, exposed options instead of dropdowns, recommended plan, auto-advance after add-to-cart, sticky CTA |
| **Persuasion lever** | A lever that works through a cognitive bias rather than by removing friction | Everything a Lever needs, PLUS a pass through the ethics gate (section 5) | urgency, scarcity, countdown timers, opt-out defaults, social proof, price framing, loss-framed copy |

A decision that is argued as a Lever but is really Hygiene wastes an experiment. A decision that is argued as Hygiene but is really a Persuasion lever skips the ethics gate. Classification is the first review comment, not the last.

## 2. The evidence ladder

Every Lever and Persuasion lever in the register carries an evidence level. The level is a property of the evidence, not of how confident the team feels.

| Level | Meaning | What it licenses |
|---|---|---|
| **E0 Intuition** | A principle or idea with no test behind it (a heuristic list, a designer's instinct, an "everyone does it") | A hypothesis. Never a reason by itself to ship a Lever on a key screen |
| **E1 Established principle** | A long-standing usability principle with research behind it (for example Nielsen's heuristics, Fitts's law, recognition over recall) | Hygiene, and a reasonable default for low-stakes screens |
| **E2 Single external test** | One published A/B test on a comparable screen | A prior. Ship on low-traffic screens with monitoring; test on high-traffic ones |
| **E3 Replicated external tests** | Several external tests in the same direction, on comparable screens, with adequate power | A strong prior. Ship with a before/after check; test only if the screen is critical or the context differs |
| **E4 Own test** | A pre-registered experiment in THIS product that met its threshold | The only level that settles a disagreement |

Downgrade rules (apply all that fit, one level each):

- **Underpowered:** the test had less than 80 percent power at a minimum detectable effect (MDE) you would care about. A "win" from an underpowered test is mostly noise; a published sample size and power figure is the first thing to read.
- **Context mismatch:** different screen type (checkout vs landing), device mix, audience (B2B vs consumer), price point, or metric (clicks vs completed purchases).
- **Proxy metric only:** the test measured a click or a visit, not the outcome you care about.
- **Single source:** the only evidence comes from the vendor selling the pattern or the tool.

Losing and flat tests count as evidence too. A pattern with two wins and three losses is not E3; it is a coin flip with a story.

## 3. The Pattern Register

The register is the project's memory of what it believes about UI levers and why. It lives in YOUR project (a section of `PROJECT_PROFILE.md` or a sibling file), not in this repo. One row per pattern, written in your own words.

| Field | Meaning |
|---|---|
| ID | UI-n, referenced from the living spec ("Frontend applies UI-4") |
| Pattern | a short name and a one-line description in your own words |
| Class | Lever / Persuasion lever (Hygiene does not need rows; it is the checklist) |
| Screen types | where it applies: landing, signup, onboarding, listing, product, pricing, cart, checkout, thank-you, in-app |
| Target metric | the metric it is expected to move, attached to a lever per `METRICS_PLAYBOOK.md` |
| Counter-metric | what must not get worse (refunds, support tickets, churn, unsubscribes, task errors) |
| Mechanism | WHY it should work, in one sentence. No mechanism, no row |
| Evidence level | E0 to E4, after downgrades, with links to the sources |
| External references | source and identifier (for example a pattern or test number from a pattern library), never copied text |
| Own results | each own test: date, screen, sample, result, decision |
| Ethics gate | pass / fail / N.A., with the date and who ran it (Persuasion levers only) |
| When NOT to use | the contexts where it fails or backfires |
| Status | candidate / adopted / rejected / forbidden |

Rules:

- **Reference, do not copy.** External pattern libraries are copyrighted and often paid. Store the identifier and link, write the description and mechanism yourself.
- **Rejected and forbidden rows stay.** A rejected pattern with its losing test is the cheapest experiment you will ever run: the one you do not repeat.
- **The register is pruned on evidence, not on fashion.** A row changes level only when new evidence arrives.

## 4. UI hygiene checklist (binary)

This extends the Frontend Definition of Done in `DELIVERY_PLAYBOOK.md`. Every line is YES / NO / N.A. per screen; QA checks it alongside the AC matrix.

- [ ] **State is visible.** Every item with a lifecycle shows its state (draft, paid, sent, failed), and selected vs clickable vs plain text are visually distinct and consistent.
- [ ] **Every action has feedback.** Success, progress, and failure are shown in place, not only in a console or a later email.
- [ ] **All four data states are designed.** Loading, empty (with the next action, not a blank), error (with a recovery path), and full / overflow.
- [ ] **Errors are caught early and forgivingly.** Inline validation at the field; inputs accept common formats (spaces in phone numbers, pasted IBANs) instead of rejecting them.
- [ ] **Mistakes are recoverable.** Undo or edit-after for reversible actions; confirmation only for truly destructive or costly ones (see the project's undo-vs-confirm rule).
- [ ] **Forms ask only what is needed now.** Every field has a stated consumer; optional fields are deferred or progressively disclosed; known data is pre-filled.
- [ ] **Labels beat guessing.** Icons that could mean two things carry a text label; field hints explain why data is asked and give an example, outside the placeholder.
- [ ] **Targets are hittable.** Touch and click targets meet the project's accessibility threshold on the smallest target device.
- [ ] **Primary action is unambiguous.** One visually dominant action per decision point; secondary actions look secondary.
- [ ] **Expectations are set.** Multi-step flows show the step and what happens next; long waits show progress.
- [ ] **Accessibility threshold met.** Contrast, focus order, keyboard path, screen-reader names, to the level in `PROJECT_PROFILE.md`.
- [ ] **Copy is plain and direct.** No jargon, no technical IDs, no hedging question marks on calls to action; the copy language and tone follow the project profile.

## 5. The ethics gate (Persuasion levers only)

Persuasion levers move numbers by working on biases. Used honestly they help people decide; used dishonestly they are dark patterns, they damage trust, and in many jurisdictions they are unlawful. SKEPTIC runs this gate in Discovery; BA re-checks it at entry in Delivery. Any NO means the pattern is forbidden in this form.

1. **True:** does the claim reflect a real constraint? A countdown must end with something actually changing; "only 3 left" must be the real stock; a "most popular" badge must be the real most popular option.
2. **Visible:** can the user see what they are getting? Defaults are stated in readable text next to the choice; opt-outs are as easy to find and read as opt-ins; no double negatives.
3. **Reversible:** can the user undo the decision without penalty and with no more effort than it took to make it (cancel as easy as subscribe)?
4. **Disclosable:** would the decision survive the user being told the mechanism ("we show a timer to make you decide faster")? If the pattern only works while hidden, it is manipulation.
5. **Lawful:** does it comply with the project's regulatory regime? Check consumer-protection and platform rules on dark patterns, and data-protection rules on consent (consent cannot be pre-ticked). The applicable regimes live in `PROJECT_PROFILE.md` section 8.

The gate also requires a counter-metric that would expose harm: refunds, chargebacks, cancellations in the first period, complaints, unsubscribes. A persuasion win with no harm detector is not a win, it is an unmeasured loan against trust.

## 6. UI experiment protocol

When a Lever needs E4, run it as an EXP test per `DISCOVERY_PLAYBOOK.md`, with these UI-specific rules.

**Hypothesis shape (fill in before building the variant):**

```
ON <screen type> FOR <segment / device>,
CHANGING <element> FROM <A> TO <B> (register row UI-n)
WILL MOVE <primary metric> FROM <baseline> BY <MDE or more>
BECAUSE <mechanism>,
WITHOUT <counter-metric> DEGRADING BEYOND <tolerance>.
DECISION RULE: ship if <threshold>, discard if <threshold>, otherwise <rule>.
```

Rules:

- **Size before you start.** Compute the sample needed for 80 percent power at the MDE that would change a decision, and the runtime that sample implies at current traffic. If the runtime exceeds the project's maximum test duration, do not run an A/B test (see the low-traffic rule below).
- **Run whole cycles.** Minimum runtime is at least one full usage cycle of the product (and in practice at least one full week), regardless of when significance first appears.
- **No peeking.** Either fix the sample in advance and read once, or use a sequential method chosen in advance. Stopping at the first green day is the most common way to manufacture a fake win.
- **Check the split.** A sample ratio mismatch (the traffic split is not what was configured) invalidates the result; fix the setup and rerun.
- **Measure the outcome, not the click.** The primary metric is the business outcome the pattern is meant to move (completed orders, activated accounts), not an intermediate click, unless the click is a declared, expiring proxy per `METRICS_PLAYBOOK.md` section 5.
- **One idea per test.** A variant may bundle several changes only when the bundle is the hypothesis; then the register records the bundle, not each part.
- **Record every result.** Wins, losses, and flats go into the register's "Own results", with the decision taken.

**Low-traffic rule.** Most screens in most products cannot reach significance in a reasonable time. For those: ship Hygiene unconditionally; ship a Lever only at E2 or better after downgrades; validate with a cheap qualitative check (a five-user usability test or a recorded session review); then monitor the metric before and after with an explicit note that a before/after comparison is not causal. Reserve real A/B tests for the few screens with the traffic and stakes to justify them.

## 7. Who uses what

- **PS / DATA (Discovery):** attach every Lever to a metric lever; DATA provides baselines and the power calculation.
- **EXP (Discovery):** draws UI hypotheses from the register, writes the hypothesis shape in section 6, runs the cheapest test that can kill it (a fake-door or a clickable mockup before a production variant).
- **PRC (Discovery):** owns price presentation patterns (anchoring, per-unit framing, recommended plan) and runs them through the ethics gate with SKEPTIC.
- **SKEPTIC (Discovery):** runs the ethics gate and attacks the evidence level ("what is the power, what is the context match").
- **BA (Delivery):** classifies the UI decisions in the Brief (Hygiene / Lever / Persuasion lever), carries the register IDs, and re-checks forbidden patterns against the project's hard rules.
- **Frontend (Delivery):** cites register IDs for every Lever applied, satisfies the hygiene checklist.
- **QA (Delivery):** checks the hygiene checklist per screen and device, including all four data states.
- **Post-release verification owner:** checks the target metric AND the counter-metric on the named date, and writes the result back into the register.

## 8. Cost scaled to risk

- **Copy tweak or a hygiene fix:** fast path. Hygiene checklist only; no register entry needed.
- **New screen or flow:** hygiene checklist, plus register IDs for any Lever used, at E1 or better.
- **Change on a revenue-critical screen (pricing, checkout, signup) or any Persuasion lever:** register entry, ethics gate if applicable, and either E3 evidence with before/after monitoring or an E4 experiment if traffic allows.

## 9. Anti-patterns

- **Cargo-cult UI:** "Amazon does it" as the whole argument. Their traffic, audience, and metric are not yours.
- **Reading a winner off an underpowered test,** or citing only the wins from a library that also lists losses.
- **A/B testing hygiene** (testing whether an error message helps) instead of shipping it.
- **Persuasion without a harm detector:** urgency or opt-out defaults with no refund, cancellation, or complaint counter-metric.
- **Fake scarcity and fake timers,** or a "recommended" badge that marks the most profitable option rather than the best fit.
- **Copying pattern descriptions** from a paid library into the project instead of referencing them.
- **Peeking and stopping early,** or redefining the primary metric after the result is in.
- **Losers deleted from the register,** so the same failed idea is tested again a year later.

## 10. External sources

Use external pattern libraries as priors, by reference:

- **GoodUI** (goodui.org): a free list of 75 conversion ideas (intuition level, E0 to E1) and a library of numbered patterns backed by contributed and leaked A/B tests, tagged by screen type, metric, and device, with sample size and statistical power published per test. Effect sizes for most tests require membership. Reference ideas and patterns by number (for example "GoodUI idea 13", "GoodUI pattern 93, test 671"), read the power figure before grading, and grade each pattern from its full test record, losses included.
- **Usability heuristics** (for example Nielsen's ten heuristics, Norman's design principles): the main E1 source for hygiene.
- **Accessibility standards** (WCAG at the level declared in the project profile): the source of truth for the accessibility line of the checklist.

---

## Adaptation to your project

Keep the specifics in `PROJECT_PROFILE.md`, leave this core clean:

- **Pattern Register location:** where the register lives and who owns it.
- **Forbidden patterns:** the Persuasion levers your product will never use, whatever a test says (for example fake timers, confirmshaming, pre-ticked add-ons).
- **Key screens and traffic:** the screens that matter for the north star and their typical weekly traffic, so the low-traffic rule can be applied without a recalculation each time.
- **Experiment tooling and limits:** the testing tool, the default MDE, the maximum test duration, and who may start a test.
- **Undo-vs-confirm rule, design system, target devices, accessibility threshold:** already in the profile's Delivery conventions; the hygiene checklist reads them from there.
