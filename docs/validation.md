# Validation Methodology

> How AI Cricket Coach is tested and audited. This document describes **process**, not proprietary thresholds or private clip names.

---

## Validation philosophy

Cricket coaching CV fails in expensive ways if the pipeline:

1. **Accepts non-cricket video** → nonsense coaching advice
2. **Rejects valid cricket video** → product unusable for real club players
3. **Detects the wrong bat** → metrics and impact timing attach to background objects
4. **Locks impact too early/late** → all downstream phase and metric timing shifts

Therefore validation is **multi-layer**:

| Layer | Question |
|-------|----------|
| Clip quality | Is the footage technically usable? |
| Cricket validator | Is this cricket batting context? |
| Object association | Is the bat/ball connected to this player? |
| Temporal logic | Is impact/phase timing coherent? |
| Metrics | Do numbers match coach review on sample clips? |
| Regression tests | Did a code change break a previously working case? |

---

## Automated regression testing

The private repository includes a pytest suite covering:

- Cricket video validator decisions and diagnostics schema
- Bat validation geometry and rejection reasons
- Bat pipeline Design B behaviour
- Nearest-bbox wrist geometry
- Torso stabilisation
- Full-body borderline recovery eligibility
- Ball tracker state transitions
- Hybrid detector contracts
- Phase detection exports
- Head-over-base metric logic
- Clip quality statuses
- Batch analyser tooling

**Verified status (August 2026):** **182 tests passing**, 0 failures.

Tests use synthetic/minimal fixtures where possible — not private video files — so they can run in CI-like environments without datasets.

---

## Known-valid clip evaluation

A batch of ** genuine club batting clips** (phone nets footage) is run through the production validator path after major changes.

Metrics tracked (conceptually):

- Pass / valid-with-warnings / reject rate
- Full-body pose percentage distribution
- Accepted bat frames and longest run
- Batter detection rate
- Movement and geometry scores
- Framing classification

Historical audits documented periods of **high false-rejection** on valid clips — motivating framing recovery and borderline full-body logic. Current rates vary by branch; the portfolio does not quote a fixed accuracy percentage.

---

## Known-invalid clip evaluation

Targeted **negative videos** test false-positive resistance:

| Category | Intent |
|----------|--------|
| Gym workout | Human + movement + bat-like equipment |
| Walking | Human + full-body + movement, no cricket |
| Baseball swing | Athletic bat motion from another sport |
| Other invalid sets | Non-batting or wrong sport context |

Checks performed:

- Final status must remain rejected
- Borderline recovery must **not** apply on negatives
- S2 strong-bat gate must **not** pass on evaluated negatives (production path)
- Rejection chain documented step-by-step

Evaluated negative batches reported **zero false accepts** under production sampling at time of audit — a encouraging but **small-sample** result.

---

## Frame-level diagnostics

For bat association failures, forensic tooling records per candidate:

- Frame index
- Bounding box and confidence
- Region test result
- Nearest wrist distance vs allowed distance
- Composite validation score
- Acceptance / rejection reason codes

Outputs include:

- CSV candidate tables
- Annotated contact-sheet images (internal — not published when they contain identifiable footage)
- Structured validation JSON on every clip run

This methodology mirrors how a production CV team debugs association failures — not only aggregate pass rates.

---

## False-positive vs false-negative trade-offs

| Tighten bat association | Effect |
|-------------------------|--------|
| ↓ false positives | ↑ false rejections on side-on / blurred bats |
| ↓ false rejections (recovery) | ↑ risk on adversarial negatives |

The project documents this tension explicitly. Safety audits simulated borderline bat-strength policies before enabling recovery paths.

---

## Production vs diagnostic comparisons

Some adversarial cases appear safe under **production sampling** but expose risk under **full-clip diagnostic replay** (e.g. swing action starting after the early validation window).

Policy: report both honestly. Do not claim validator robustness against baseball-bat swings based only on window-missed negatives.

---

## Human metric validation (in progress)

A separate workflow supports coach review of metric outputs against labelled expectations:

- Review CSVs and evidence images
- Pass/fail/warn per metric per clip
- Aggregate summaries

This human loop is **started but not complete** — automated tests do not replace biomechanics expert sign-off.

---

## What we do not claim

- No fixed "99% accurate" headline
- No production deployment SLA
- No benchmark leaderboard comparison against unpublished industry systems
- No guarantee all club phone footage will pass validation

---

## Related documents

- [`engineering-challenges.md`](engineering-challenges.md)
- [`roadmap.md`](roadmap.md)
