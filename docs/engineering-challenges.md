# Engineering Challenges

> Case studies from real investigation work on AI Cricket Coach.  
> Summaries are high-level — exact thresholds, proprietary scoring, and internal clip identifiers are omitted.

---

## Case Study 1 — Net pole and railing false-positive bats

### Problem

YOLO frequently detected **bat-shaped background objects** — net poles, railings, rods, and fence elements — particularly in busy nets environments. Raw detections looked plausible by confidence alone.

### Investigation

- Frame-level bat association debug JSON
- Rejection-reason histograms (`OUTSIDE_BATTER_REGION`, wrist-distance failures)
- Side-by-side contact sheets of accepted vs rejected candidates
- Comparison of single-model vs hybrid + pose-aware validator paths

### Root cause

Cricket bats share visual features with vertical linear structures. A confidence threshold cannot distinguish **a bat in the player's hands** from **a pole near the camera**.

### Approach

Implemented **Design B bat pipeline**:

1. Preserve all raw YOLO bat candidates per frame
2. Run pose-aware `BatValidator` on every candidate
3. Select at most one accepted bat using spatial region + wrist proximity + composite score
4. Reject when wrist evidence is unavailable rather than guessing

Additional signals: expanded batter box, torso-normalised wrist distance, temporal accepted-bat runs for clip-level validation.

### Result

Material reduction in false bat acceptance on difficult nets clips. Engineering audits document cases where railing-dominated detections dropped to **zero accepted bat frames** while genuine cricket clips retained strong bat evidence.

### Remaining limitations

- Low-confidence true bats on side-on or blurred swings still missed
- Background bat-like objects inside the expanded player region remain adversarial
- YOLO class confusion on non-cricket bat-shaped objects (e.g. baseball bats) depends on sampling window and confidence

---

## Case Study 2 — Bat bounding-box centre vs wrist geometry

### Problem

Valid cricket clips were rejected because the **centre** of a long thin bat bounding box sat far from the wrists, even when the handle was near the hands.

### Investigation

- Per-frame CSV of wrist positions vs bat box
- Diagnostic overlays showing centre-distance vs nearest-edge distance
- Before/after comparison on a side-on clip with low accepted-bat rate

### Root cause

Cricket bats are **elongated**. Centre-point geometry penalises vertical and side-on bats incorrectly.

### Approach

Investigated and implemented **nearest bounding-box point** wrist distance:

- Measure distance from wrists to closest point on bat rectangle
- Normalise allowed distance by torso scale
- Combine with region and confidence terms in a composite validation score

Documented in internal audit reports; evaluated on regression clips.

### Result

Accepted bat frame counts improved substantially on affected side-on footage (order-of-magnitude recovery on a documented regression case).

### Remaining limitations

- Extreme follow-through frames may still place bat tip outside region
- Missing wrist landmarks require conservative rejection paths
- Torso-scale errors propagate into distance gates

---

## Case Study 3 — False rejection of valid cricket videos (full-body gate)

### Problem

The cricket video validator rejected genuine batting clips that had strong cricket evidence because a **single full-body pose percentage** sat marginally below a hard threshold — sometimes due to MediaPipe threshold-edge noise rather than truly bad framing.

### Investigation

- Multi-run reproducibility audit showing 1-frame flips near gate boundary
- Batch evaluation on known-valid club footage with false-rejection counts
- Component-score breakdown (bat, batter, movement, geometry, ball track)

### Root cause

Binary clip gate on a noisy pose metric **overruled** independent cricket signals (strong bat runs, movement, geometry).

### Approach

Implemented **evidence-based borderline recovery**:

- Normal path: full-body above primary threshold
- Borderline band: require multiple independent signals (strong batter, strong accepted bat evidence, movement, geometry, framing safety)
- Strong bat evidence uses higher frame-count and percentage requirements than ordinary minimums (S2-style safety)

### Result

Recovered specific borderline valid clips in regression testing while targeted negatives (gym, walking) remained rejected. Calibration continues.

### Remaining limitations

- MediaPipe non-determinism on edge frames
- Late-start swings may fall outside the validator's early sampling window
- Recovery must not become a false-positive bypass — ongoing adversarial negative testing

---

## Case Study 4 — Pose instability and torso-scale collapse

### Problem

Wrist-distance gates misfired when:

- Wrist visibility dropped intermittently
- Shoulder–hip scale collapsed on a single bad pose frame
- Landmark jitter shifted torso normalisation

### Investigation

- Torso-scale timeline plots
- Stabilisation event counts in validation JSON
- Frame captures at pose-collapse moments

### Approach

- Landmark stabilisation for key points
- Torso-scale history with collapse detection
- Reject bat candidates when wrist evidence is unavailable (missing-wrist safety)
- Batter lock to reduce identity switches

### Result

Reduced erratic accept/reject flipping on noisy pose sequences. Documented as partial — not all collapse modes eliminated.

### Remaining limitations

- Fast motion blur still causes dropout
- Portrait phone clips with unreliable ankles affect full-body scoring
- Single-frame pose collapse can still influence clip-level percentages

---

## Case Study 5 — Targeted negative testing (non-cricket footage)

### Problem

Need confidence that validation improvements do not accept gym workouts, walking videos, or bat-like swing motions from other sports.

### Investigation

Curated negative set including:

- Gym workout (human + movement + bat-like equipment detections)
- Walking (human + full-body + movement, no bat)
- Baseball swing (athletic stance + bat-shaped object + strong movement)

Production validator path + full-clip diagnostic forensics.

### Root cause (production path)

Negatives rejected via different mechanisms:

| Negative type | Primary block |
|---------------|---------------|
| Gym | Raw bat false positives rejected at association; 0% batter |
| Walking | No bat pipeline activity |
| Baseball (late swing) | Early 2s validation window misses swing; 0 accepted bats in production sample |

### Approach

Document rejection chains, S2 strong-bat safety, borderline recovery eligibility, and frame-level bat candidate CSVs **without changing production thresholds during audit**.

### Result

Zero false accepts on evaluated negative set under production sampling. Diagnostic work flagged **sampling blind spot** when swing action occurs after the validation window — listed as future test priority, not hidden.

### Remaining limitations

- Small negative sets do not prove broad safety
- Adversarial cases with real cricket bat + non-batting context still needed
- Window alignment research ongoing

---

## Themes a CV engineer should notice

1. **Failure analysis drove design** — not only metric chasing on training mAP
2. **Object detection + pose fusion** — standard boxes alone were insufficient
3. **Clip-level validation is a system** — gates interact; fixing one metric exposed others
4. **Forensic tooling** — debug JSON, contact sheets, batch analysers treated as first-class engineering
5. **Honest limits** — ball tracking, late-start windows, and side-on geometry still open

---

## Related documents

- [`validation.md`](validation.md)
- [`computer-vision-pipeline.md`](computer-vision-pipeline.md)
