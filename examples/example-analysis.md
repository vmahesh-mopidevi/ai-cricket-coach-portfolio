# Example Analysis Walkthrough

> **Illustrative walkthrough** showing how AI Cricket Coach connects CV evidence to coaching language.  
> Values below are **placeholders** unless you replace them with anonymised real output.

---

## Input

| Field | Example |
|-------|---------|
| Source | Smartphone video, front-on, nets session |
| Duration | ~3 seconds batting clip |
| Resolution | 1080p portrait or landscape |
| Player | Anonymised club batter — no identifying info in public materials |

---

## Stage 1 — Clip quality pre-check

```text
Status: PASS_WITH_WARNING
Note:   Ankle landmarks intermittently low visibility (common on portrait phone clips).
Action: Analysis continues.
```

---

## Stage 2 — Cricket video validation

```text
Status:     VALID_WITH_WARNINGS
Confidence: [placeholder — e.g. 0.72]

Signals observed (conceptual):
  ✓ Full-body pose sufficient
  ✓ Movement score above threshold
  ✓ Pose geometry plausible (front-on)
  ✓ Accepted bat evidence present across multiple frames
  ✓ Confirmed moving ball track (when ball visible)
  ⚠ Minor framing warning — subject slightly small in frame
```

If validation fails, the pipeline stops here and returns structured rejection reasons (e.g. no bat evidence, invalid geometry, extreme close-up).

---

## Stage 3 — Detected player and objects

```text
Batter:  YOLO box + pose landmarks tracked across frames
Bat:     Accepted on N frames (pose-associated validation)
Ball:    Tracked — state CONFIRMED on M frames (when visible)
```

*(Replace N/M with real counts only from anonymised internal runs — do not invent.)*

---

## Stage 4 — Pose and stabilisation

MediaPipe landmarks → stabilised wrists, shoulders, hips, ankles.

Used for:

- Biomechanics metrics
- Bat–wrist association
- Movement score for phases

---

## Stage 5 — Shot phase and impact

```text
Impact frame:   [placeholder — e.g. frame 212]
Impact method:  Contact-zone window + minimum bat–ball distance

Legacy phases (example):
  stance      → frames 1–45
  backlift    → frames 46–120
  downswing   → frames 121–211
  impact      → frame 212
  follow-through → frames 213–end
```

Phase boundaries depend on clip length and detection quality — treat numbers as illustrative.

---

## Stage 6 — Biomechanics metrics (front-on example)

```text
Metric                  Result (illustrative)
─────────────────────  ─────────────────────────
Head Over Base          WARN — head outside support base before impact
Stance Width Ratio      PASS — within expected band
Shoulder Tilt           PASS
Hip Tilt                PASS
Body Lean               WARN — drifting toward off side in downswing
Balance Score           72 / 100
```

---

## Stage 7 — Coaching feedback

```markdown
# Batting Analysis

## Headline
Main focus: keep your head inside your support base until after contact.

## Observations
1. **Head Over Base** — Your head moved outside your support base before impact.
2. **Body Lean** — Torso drifted during the downswing.

## Why it matters
Early head movement reduces control and can cause inconsistent contact point.

## Suggested drill
Shadow batting: 3 × 10 reps focusing on a fixed head position through contact.
```

---

## Pipeline diagram (this clip)

```mermaid
flowchart LR
    IN[Phone video] --> Q[Quality PASS]
    Q --> V[Validator VALID]
    V --> D[YOLO + Pose]
    D --> I[Impact @ frame 212]
    I --> P[Phases labelled]
    P --> M[Metrics computed]
    M --> C[Coaching markdown]
```

---

## How to replace placeholders safely

1. Run the private pipeline on a **fully anonymised** clip (no face identification, no venue logos, no third-party copyright).
2. Copy **structure** of validation JSON and coaching markdown — redact file paths and clip names.
3. Mark the document: *"Example from anonymised internal run, [date]."*
4. Never publish annotated MP4 unless rights and privacy are cleared.

---

## Related

- [Main README](../README.md)
- [Architecture](../docs/architecture.md)
