<table>
<tr>
<td width="180"><img src="asset/Logo.png" alt="Privatect logo" width="220"/></td>
<td>

# Privatect — On‑Device Privacy Studio  
### Detect • Mask • Share — with confidence

</td>
</tr>
</table>

---

## 🚨 Problem

Modern sharing workflows (screenshots of chats/dashboards, audios, captions, prompts sent to LLMs) routinely leak:
- Bystander faces and creator identity linkages
- Emails, phone numbers, IDs, addresses, credentials
- Internal project names, partial tokens, API keys (in screenshots / prompts)
- Street & shop signage, license plates (location inference)
- Screen overlays (notifications, chat bubbles) captured unintentionally

Manual blurring is slow, inconsistent, and error-prone; cloud redaction services introduce a second exposure surface (logging, retention, model retraining). Privatect brings a multilayer privacy firewall directly onto your device.

---

## ✨ Privatect At a Glance
<p align="center">
  <img src="asset/demo.png" width="100%"/>
</p>
<p align="center">
  <img src="asset/demo.gif" width="70%" alt="Sentinel demo GIF"/>
</p>
Privatect is your pre‑publish privacy shield: before you share a screenshot, send a prompt to an external AI, export a support log, or package a research dataset, it sweeps your content for the usual suspects (faces, emails, phone numbers, IDs, screen text, signage, chat bubbles) and less obvious contextual clues (location hints, internal labels), then lets you instantly anonymize them with the style you prefer—fast automatic cleanup for quick sharing, or a deeper semantic pass when you need thorough assurance. All on‑device.

## 📦 Setup

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Download Required Models
Download the PII detection models from your course-provided storage or your own artifacts.

Available models:
- `pii_deberta_base_v1` – DeBERTa-based PII token classifier (default)
- Optional: `pii_ettin_encoder_*`, `pii_modernbert_base_v1` if you trained them

Download the Qwen2.5vl-3B model from: [https://huggingface.co/Qwen/Qwen2.5-VL-3B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-3B-Instruct)

### 3. Set Up YOLO (Face)

MacOS
```bash
curl -L -o yolov12l-face.pt "https://github.com/YapaLab/yolo-face/releases/download/v0.0.0/yolov12l-face.pt"

mkdir -p ~/.insightface/models

# Download InSwapper model
gdown "https://drive.google.com/uc?id=1krOLgjW2tAPaqV-Bw4YALz0xT5zlb5HF" \
  -O ~/.insightface/models/inswapper_128.onnx
```

Windows:
```bash
Invoke-WebRequest "https://github.com/YapaLab/yolo-face/releases/download/v0.0.0/yolov12l-face.pt" -OutFile "yolov12l-face.pt"

mkdir .insightface\models

gdown "https://drive.google.com/uc?id=1krOLgjW2tAPaqV-Bw4YALz0xT5zlb5HF" `
  -O ".insightface\models\inswapper_128.onnx"
```

---

## 🚀 Run & Test

Test media (multi-face images, signage, screen snippets) live with following commands:

### macOS / Linux
```bash
python3 face_anonymizer_gui_modern.py
```

### Windows
```powershell
python face_anonymizer_gui_modern.py
```

Try inside the GUI:
- Switch Fast ↔ Deep Mode
- Toggle anonymization styles (Blur / Pixelate / Emoji / Synthetic)
- Keep largest face (retain creator identity)
- Interactive face selection (click faces to include/exclude)
- Load signage / screen / chat images to see text region fusion

### Text Privacy
- PII Summary: show entities for a snippet
- Process: direct in‑place masking to Output Area (uses "xxxx")
### Video
- New Video tab: face masking for videos (Blur/Pixelate) with progress bar

---

## 💠 Core Features & Advantages

### Core Features
1. 100% On‑Device Processing  
   All detection & anonymization run locally—no uploads, no server round‑trips.

2. Dual Modes (Fast / Deep)  
   Fast Mode: instant face + obvious region cleanup.  
   Deep Mode: adds OCR + contextual + semantic PII sweep for maximum coverage.

3. Multimodal Coverage (Now)  
   Faces, textual PII (58+ classes), screen / signage / chat and audio.

4. Visual Anonymization Styles  
   Blur (Gaussian/Box/Median), Pixelate, Emoji overlay, High‑fidelity Synthetic Face Swap (InSwapper).

5. Text Privacy Engine  
   Structured entity tagging.

6. Bystander & Creator Controls  
   Keep largest face (retain presenter) or anonymize everyone; interactive include/exclude selection.

### Key Advantages
- Zero Secondary Exposure: Eliminates cloud logging & retention risks.
- Semantic Depth: Goes beyond plain OCR—catches contextual & implicit sensitive cues.
- Fast-to-Use: GUI + one-click mode switching encourages habitual pre‑publish scrubbing.
- Aesthetic Flexibility: Choose the anonymization style that matches your brand or compliance needs.
- Compliance Ready: Deterministic policies + audit manifest support internal review & documentation.
---

## 🧱 Tech Stack (Key libraries only)

### Computer Vision & Image Processing
ultralytics  
insightface  
onnxruntime  
opencv-python  
Pillow  
albumentations  
scikit-image  

### ML / NLP / PII
torch  
torchvision  
transformers  
huggingface-hub  
openai-whisper  
scikit-learn  
safetensors  
tiktoken  

### Audio / Speech
PyAudio  
Whisper  

### GUI / UX
tkinter  
customtkinter  
tkinter-tooltip  

### Data / Performance
numpy  
numba  

### OCR
EasyOCR

## 🛡️ Privacy & Ethical Stance

- No re-identification or inversion tooling  
- Irreversible transformations (configurable but protective defaults)  
- All processing in volatile local scope; nothing phoned home  
- Manifest export optional & user-controlled  
- Focus on minimizing false negatives while allowing manual override for edge cases  

---

## ⚠️ Notes

- This is a CS3263 course project prototype intended for educational use.  
- Functionality is experimental and may contain inaccuracies or over‑masking. Please review outputs before distribution.  
- Third‑party models and datasets are used under their respective licenses.  

---

## 📚 Links

- PII model training notes: `pii-text-detector(train)/README.md`

---

## 📑 Citations
```
@misc{weller2025seqvsseqopen,
  title         = {Seq vs Seq: An Open Suite of Paired Encoders and Decoders},
  author        = {Orion Weller and Kathryn Ricci and Marc Marone and Antoine Chaffin and Dawn Lawrie and Benjamin Van Durme},
  year          = {2025},
  eprint        = {2507.11412},
  archiveprefix = {arXiv},
  primaryclass  = {cs.CL},
  url           = {https://arxiv.org/abs/2507.11412}
}
```
```
@misc{modernbert,
  title         = {Smarter, Better, Faster, Longer: A Modern Bidirectional Encoder for Fast, Memory Efficient, and Long Context Finetuning and Inference},
  author        = {Benjamin Warner and Antoine Chaffin and Benjamin Clavié and Orion Weller and Oskar Hallström and Said Taghadouini and Alexis Gallagher and Raja Biswas and Faisal Ladhak and Tom Aarsen and Nathan Cooper and Griffin Adams and Jeremy Howard and Iacopo Poli},
  year          = {2024},
  eprint        = {2412.13663},
  archiveprefix = {arXiv},
  primaryclass  = {cs.CL},
  url           = {https://arxiv.org/abs/2412.13663}
}
```
```
@misc{he2021debertav3,
  title         = {DeBERTaV3: Improving DeBERTa using ELECTRA-Style Pre-Training with Gradient-Disentangled Embedding Sharing},
  author        = {Pengcheng He and Jianfeng Gao and Weizhu Chen},
  year          = {2021},
  eprint        = {2111.09543},
  archiveprefix = {arXiv},
  primaryclass  = {cs.CL}
}
```
```
@inproceedings{he2021deberta,
  title     = {DEBERTA: DECODING-ENHANCED BERT WITH DISENTANGLED ATTENTION},
  author    = {Pengcheng He and Xiaodong Liu and Jianfeng Gao and Weizhu Chen},
  booktitle = {International Conference on Learning Representations},
  year      = {2021},
  url       = {https://openreview.net/forum?id=XPZIaotutsD}
}
```
```
@misc{ai4privacy_2023,
  author     = {ai4Privacy},
  title      = {pii-masking-200k (Revision 1d4c0a1)},
  year       = {2023},
  url        = {https://huggingface.co/datasets/ai4privacy/pii-masking-200k},
  doi        = {10.57967/hf/1532},
  publisher  = {Hugging Face}
}
```
```
@article{Qwen2.5-VL,
  title={Qwen2.5-VL Technical Report},
  author={Bai, Shuai and Chen, Keqin and Liu, Xuejing and Wang, Jialin and Ge, Wenbin and Song, Sibo and Dang, Kai and Wang, Peng and Wang, Shijie and Tang, Jun and Zhong, Humen and Zhu, Yuanzhi and Yang, Mingkun and Li, Zhaohai and Wan, Jianqiang and Wang, Pengfei and Ding, Wei and Fu, Zheren and Xu, Yiheng and Ye, Jiabo and Zhang, Xi and Xie, Tianbao and Cheng, Zesen and Zhang, Hang and Yang, Zhibo and Xu, Haiyang and Lin, Junyang},
  journal={arXiv preprint arXiv:2502.13923},
  year={2025}
}
```
<p align="center"><b>Privatect — Share boldly. Reveal wisely.</b></p>
