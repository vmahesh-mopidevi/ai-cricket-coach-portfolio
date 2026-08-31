# Assets Guide

> Public portfolio visuals. **Do not copy private-repo evidence images automatically** — many contain real players, venues, or copyrighted footage.

---

## Published demo assets

The following files are included in this repository and referenced from the README:

| File | Description |
|------|-------------|
| [`demo.gif`](demo.gif) | ~12 s portfolio demo (900×506, optimised for GitHub) |
| [`demo-preview.png`](demo-preview.png) | Static preview frame (1280×720) |
| [`ai-cricket-coach-demo.mp4`](ai-cricket-coach-demo.mp4) | Higher-quality MP4 demo (1280×720, H.264) |

These show a **sanitised, public-safe** end-to-end analysis example: pose estimation, object detection, temporal impact analysis, biomechanics metrics and coaching feedback.

The source annotated video, model weights, validation JSON and production codebase remain private.

---

## Optional future enhancements

The portfolio is publishable without the items below. Add only if they improve clarity and pass the anonymisation checklist.

### Architecture diagram

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/architecture-diagram.png` |
| **Content** | Recreate the Mermaid diagram from README as a clean static PNG/SVG |
| **Safety** | Diagram only — no video frames |

### Pose estimation overlay

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/pose-overlay.jpg` |
| **Content** | Single frame with MediaPipe skeleton on anonymised batter |
| **Safety** | Must be anonymised; no identifiable minors without consent |

### Batter / bat / ball detection overlay

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/detection-overlay.jpg` |
| **Content** | YOLO boxes for batter, bat, ball with distinct colours |
| **Safety** | Same anonymisation as pose overlay |

### False-positive rejection example

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/false-positive-rejection.jpg` |
| **Content** | Frame or diagram where a pole/railing detection is **rejected** |
| **Alternative** | Recreate synthetic diagram instead of real frame if anonymisation is hard |

### Biomechanics metric visualisation

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/metrics-chart.png` |
| **Content** | Phase boundary or movement-score plot (chart only, no video frame) |

### Coaching report screenshot

| Field | Guidance |
|-------|----------|
| **Filename** | `assets/coaching-report.png` |
| **Content** | Markdown-rendered coaching output (headline + observations + drill) |
| **Safety** | Text only — use illustrative values or redacted real report |

---

## Placeholders directory

`assets/placeholders/` contains text stubs for optional visuals not yet added. The demo GIF placeholder has been removed now that `demo.gif` exists.

---

## Anonymisation checklist

Before publishing any additional frame or GIF:

- [ ] Face blurred or clip filmed from behind / non-identifiable distance
- [ ] No venue logos, school names, or academy branding
- [ ] No third-party professional player footage
- [ ] No filenames or paths visible in overlay text
- [ ] No validation JSON with internal clip IDs in screenshot
- [ ] Rights confirmed for music/background if present

---

## What NOT to add

- Model weight files (`.pt`, `.pth`, `.onnx`)
- Training dataset images from Roboflow exports
- Full unannotated source videos beyond the approved public demo
- Evidence contact sheets from negative audits (gym/baseball real footage)
- Debug JSON with local filesystem paths
- Raw confidence values or internal frame identifiers in screenshots

---

## Optional code sample policy

If you later want to publish a tiny code excerpt (e.g. nearest-bbox geometry concept):

1. Ask whether the snippet exposes core IP
2. Prefer pseudocode in docs over raw source
3. Keep thresholds out of public snippets

No code samples are included in this portfolio by default.
