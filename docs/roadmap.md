# Roadmap

> Separating **current capabilities** from **planned research**. Items below marked *future* are not implemented unless explicitly stated in the main README.

---

## Near term

| Area | Goal | Status |
|------|------|--------|
| Ball detection recall | Improve small-ball detection on phone footage | Partial — active iteration |
| Validation calibration | Reduce false rejections without opening false positives | Ongoing |
| Camera-angle routing | Reliable front-on vs side-on metric selection | Partial — geometry classifier exists |
| Late-start swing testing | Audit validation window blind spots | Investigated — documented risk |
| Additional negative tests | Broom/stick swings, walking with cricket bat, etc. | Planned test matrix |
| Human metric sign-off | Coach review workflow completion | In progress |
| Anonymised public demo | Safe portfolio GIF and screenshots | Pending manual assets |

---

## Medium term

| Area | Notes |
|------|-------|
| **Multi-object tracking** | Associate ball and players across frames beyond current ball tracker — *future*; ByteTrack/DeepSORT **not yet used** |
| **Ball trajectory estimation** | Path reconstruction after improved detection — *future* |
| **Shot classification** | Drive, cut, pull, defensive — *future* |
| **Learned temporal models** | TCN or lightweight action model for phases — *future*; currently rule-based |
| **ONNX export** | Portable inference graph — *future* |
| **GPU inference optimisation** | Batch / TensorRT-class speedups — *future*; development today is local CPU/GPU laptop |
| **FastAPI analysis service** | Structured REST API over pipeline — *future*; stub file exists, empty |
| **Streamlit review UI** | Internal clip review — *future*; stub only |

---

## Research direction

Longer-horizon investigations aligned with sports-AI and multimodal coaching:

| Direction | Description |
|-----------|-------------|
| **Vision-language models** | Explain *why* a metric matters in natural language grounded in evidence frames — *research* |
| **Multimodal coaching** | Combine video, metrics, and conversational follow-up — *research* |
| **Personalisation** | Player baseline comparison over time — *research* |
| **Mobile on-device inference** | Reduced models for pitch-side feedback — *research* |
| **Advanced biomechanics validation** | 3D pose or multi-camera fusion — *research* |
| **Event detection benchmarks** | Structured evaluation against labelled impact/shot events — *research* |

---

## Explicit non-claims

The following appear in some job descriptions but are **not** current experience in this repository:

- ByteTrack / DeepSORT / StrongSORT production use
- NVIDIA DeepStream / TensorRT deployment
- Kubernetes / cloud MLOps infrastructure
- AWS / Azure / GCP production serving
- VLM / LangChain production deployment
- Vector-database RAG coaching systems

These may be listed here as **alignment targets** for future work, not as completed features.

---

## Maturity snapshot

| Maturity level | Components |
|----------------|------------|
| **Research prototype** | End-to-end CLI pipeline, coaching markdown, validation JSON |
| **Engineered subsystems** | Bat validator, ball tracker, cricket validator, impact FSM |
| **Incomplete** | Web API, mobile app, human metric certification, trajectory analytics |
| **Not started** | Production deployment, subscription product, academy integrations |

---

## Related documents

- [Main README](../README.md)
- [`architecture.md`](architecture.md)
- [`validation.md`](validation.md)
