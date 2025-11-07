# Privatect — Final Report

## 1. Project Overview

Privatect is an on‑device privacy studio that detects and masks personally identifiable information (PII) across text, images, and video. The system targets pre‑publication workflows: before sharing a screenshot, posting a video, or sending a prompt externally, Privatect sweeps content for sensitive items (faces, emails, phone numbers, addresses, IDs) and applies configurable anonymization.

Deliverables include a desktop GUI with three main tabs:
- Image: Face anonymization (Blur, Pixelate, Emoji, optional Synthetic swap)
- Text: PII entity detection and direct masking
- Video: Face masking (Blur/Pixelate) with progress reporting

All core processing is local. No data leaves the machine.

## 2. Requirements Mapping

- On‑device processing: satisfied via PyTorch/Transformers and Ultralytics YOLO with local weights
- PII detection for text: DeBERTa token classifier fine‑tuned for token‑level NER classes
- Face anonymization: YOLO face detection + masking operators (blur/pixelate/emoji); optional InSwapper integration for synthetic replacement (local ONNX model)
- Simple GUI: implemented with CustomTkinter; provides intuitive controls and inline toolbars
- Reproducible setup: requirements.txt and clear model placement instructions

## 3. System Architecture

- GUI (CustomTkinter): `face_anonymizer_gui_modern.py`
  - Image tab -> YOLO detector + anonymizers
  - Text tab -> DeBERTa detector (`run_pii_deberta.py`), two flows:
    - PII Summary (popup with entities)
    - Process (direct masking to Output Area)
  - Video tab -> YOLO detector per frame, applies blur/pixelate, writes MP4

- Models
  - Face: `yolov12l-face.pt` (Ultralytics). Architecture + config embedded in `.pt` (no extra cfg).
  - Text: local HF folder `pii_deberta_base_v1/final` with `config.json`, tokenizer files, and weights.
  - Optional: InSwapper ONNX for high‑fidelity face swap: `~/.insightface/models/inswapper_128.onnx`.

## 4. Implementation Details

- Text PII Detection (`run_pii_deberta.py`)
  - Loads fine‑tuned DeBERTa as `AutoModelForTokenClassification` + pipeline(ner, aggregation_strategy="simple").
  - Robust local path resolver: tries common folder layouts and surfaces clear errors.
  - CLI test harness and a minimal standalone GUI for quick validation.

- GUI Enhancements (`face_anonymizer_gui_modern.py`)
  - Branding updated to Privatect (window title + header).
  - Text tab toolbars embedded directly in the Input/Output headers with tooltips.
  - Direct masking uses constant token `xxxx` and inserts spaces where needed; email regex fallback.
  - Video tab integrates detector reuse, background processing, status, and progress bar.

- Image/Video Face Pipelines (`face_anonymizer_main.py`, helpers)
  - YOLO face detection → face bounding boxes
  - Anonymizers: blur (gaussian/box/median with tunable strength), pixelate (block size/quality), emoji overlay, optional synthetic face replacement via InSwapper or classic resize/paste fallback.

## 5. Evaluation (Qualitative)

- Text: On curated sentences, DeBERTa detects common PII (names, phones, SSNs, addresses). Direct masking is deterministic and reversible only with the original—no leakage.
- Faces: YOLO weights provide robust detections in typical lighting; pixelate/blur are fast and visually irreversible for casual observers. Video processing maintains temporal consistency with per‑frame detection.
- Performance: On CPU‑only laptops, pixelate/blur video runs in real time for SD/HD depending on hardware; smaller YOLO variants can improve throughput if needed.

## 6. Privacy, Ethics, and Limitations

- All processing is local; no cloud calls.
- Over‑ or under‑masking can occur depending on thresholds and model limitations. Users should review outputs.
- Synthetic replacement is intended for privacy preservation; it should not be used for impersonation or deception.
- Regex fallbacks exist for emails; more patterns (phones, IDs) can be added to strengthen recall when the model misses edge cases.

## 7. Setup & Usage

1) `pip install -r requirements.txt`
2) Place models:
   - `yolov12l-face.pt` in project root
   - `pii_deberta_base_v1/final/` with `config.json`, tokenizer files, and weights
   - Optional: `~/.insightface/models/inswapper_128.onnx` for synthetic replace
3) Run GUI: `python face_anonymizer_gui_modern.py`

## 8. Future Work

- Add optional lighter YOLO face weights and a model selector in the GUI
- Expand video modes to Emoji and Synthetic face replacement
- Add regex fallbacks for phones, IDs, and addresses
- Export redaction manifests and audit trails
- Add batched frame processing or tracking to reduce per‑frame detection cost

## 9. Conclusion

Privatect demonstrates a practical, on‑device approach to privacy‑preserving preprocessing across text, images, and video. The modular design and local‑first constraints make it suitable for personal use and as a foundation for further research and productization.

