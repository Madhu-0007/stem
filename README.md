# stem
# STEM to Sign Language Translator

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0.3-black?logo=flask)
![Groq](https://img.shields.io/badge/LLM-Groq%20Llama%203.3-orange?logo=meta)
![SIGML](https://img.shields.io/badge/Animation-SIGML-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-Render-46E3B7?logo=render&logoColor=white)](https://stem-1-6y4g.onrender.com/)

**A STEM-optimized sign language translation platform that converts English text, mathematical expressions, and scientific formulas into animated Indian Sign Language (ISL) or American Sign Language (ASL).**

*Built for HackX 2026 — bridging the gap between STEM education and the Deaf community.*

</div>

---

## 🔗 Live Demo

🌐 **[https://stem-1-6y4g.onrender.com/](https://stem-1-6y4g.onrender.com/)**

> [!NOTE]
> The app is hosted on Render's **free tier**, which spins down after inactivity. On first visit, **please wait 2–3 minutes** for the server to wake up. The page will load automatically — just be patient! ☕

---

## ✨ Features

| Feature | Description |
|---|---|
| 🤟 **Dual Sign Language** | Full support for both ISL (SOV grammar) and ASL (SVO grammar) via AI grammar engines |
| 🔬 **STEM Formula Parsing** | Converts 50+ chemical formulas, physics equations, and algebraic identities into sign gloss |
| 🧠 **LLM-Powered Gloss** | Uses **Groq (Llama 3.3-70B)** to intelligently restructure sentences to grammatically correct sign gloss |
| 🎭 **SIGML Avatar** | Outputs animated 3D signing avatar driven by SIGML (Signing Gesture Markup Language) files |
| 📄 **Document Upload** | Accepts PDF, DOCX, PPTX, images (PNG/JPG/BMP/TIFF/WEBP) and extracts text via OCR |
| 🗂️ **Translation History** | Stores past translations for quick replay and review |
| ⚡ **GPU Acceleration** | Auto-detects CUDA (NVIDIA GPU) for faster NLP processing via PyTorch |
| 🌐 **CORS-Ready API** | REST API endpoints usable from any frontend or client |

---

## 🏗️ Architecture

```
User Input (Text / File Upload)
        │
        ▼
┌─────────────────────┐
│  Text Extraction    │  ← PDF, DOCX, PPTX, Image (OCR via EasyOCR)
│  (utils/extraction) │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  STEM Preprocessor  │  ← Formulas, Greek letters, math ops, units
│  (preprocess_math)  │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Groq LLM Engine   │  ← llama-3.3-70b converts English → Sign Gloss
│  (llm_to_gloss)    │     (ISL: SOV order, ASL: SVO order)
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  SIGML Matcher      │  ← Maps gloss words → SIGML animation files
│  (match_to_sigml)  │     Synonyms, suffix stripping, fingerspelling fallback
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Signing Avatar UI  │  ← CWA SIGML Player renders 3D animations
│  (templates/index)  │
└─────────────────────┘
```

---

## 🧪 STEM Support

### Mathematical Expressions
Algebraic identities, exponents, and operators are automatically expanded:
```
(a+b)²  →  OPEN BRACKET A PLUS B CLOSE BRACKET SQUARE
e=mc²   →  E EQUAL M C POWER TWO
```

### Physics Formulas (20+ recognized)
| Formula | Expansion |
|---|---|
| `F=ma` | F EQUAL M A |
| `E=mc²` | E EQUAL M C POWER TWO |
| `V=IR` | V EQUAL I R |
| `PV=nRT` | P V EQUAL N R T |
| `KE=½mv²` | K E EQUAL ONE DIVIDE TWO M V POWER TWO |

### Chemical Formulas (50+ recognized)
| Formula | Expansion |
|---|---|
| `H₂O` | H TWO O |
| `CO₂` | C O TWO |
| `H₂SO₄` | H TWO S O FOUR |
| `C₆H₁₂O₆` | C SIX H TWELVE O SIX |

### Greek Letters
`α β γ δ ε θ λ μ σ τ φ ω` → `ALPHA BETA GAMMA DELTA ...`

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.10+**
- **Groq API Key** — [Get one free at console.groq.com](https://console.groq.com)
- **NVIDIA GPU** (optional) — CUDA acceleration auto-detected via PyTorch

### Installation

1. **Clone the repository**
   ```sh
   git clone https://github.com/ArekatlaNishanthchowdary/STEM-to-Sign-Language.git
   cd STEM-to-Sign-Language
   ```

2. **Create a virtual environment**
   ```sh
   python -m venv venv

   # Windows
   .\venv\Scripts\Activate.ps1

   # Linux / macOS
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```sh
   pip install -r requirements.txt
   ```

4. **Configure your API key**

   Create a `.env` file in the project root:
   ```env
   GROQ_API_KEY=your_groq_api_key_here
   ```

### Running Locally

```sh
python main.py
```

Open your browser and go to: **[http://127.0.0.1:5000](http://127.0.0.1:5000)**

---

## 🌐 Deployment (Render)

This project is configured for one-click deployment on [Render](https://render.com).

1. Push your code to GitHub.
2. Connect the repository on [render.com/new](https://render.com/new).
3. Render will auto-detect `render.yaml` and configure the service.
4. Add your `GROQ_API_KEY` as an **Environment Variable** in the Render dashboard.
5. Deploy — the `Procfile` uses `gunicorn` for production serving.

**Procfile** (already configured):
```
web: gunicorn main:app
```

---

## 📁 Project Structure

```
STEM-to-Sign-Language/
├── main.py                 # Core Flask app — NLP, LLM, routing
├── app.py                  # App entry point
├── templates/
│   └── index.html          # Frontend UI with SIGML avatar player
├── static/
│   └── SignFiles/          # 1400+ SIGML animation files
├── utils/
│   ├── extraction.py       # Document & image text extraction (OCR)
│   └── __init__.py
├── words.txt               # Vocabulary list of available sign words
├── requirements.txt        # Python dependencies
├── render.yaml             # Render deployment config
├── Procfile                # Gunicorn startup command
└── .env                    # API keys (not committed to Git)
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Backend** | Flask 3.0, Python 3.10+ |
| **LLM / NLP** | Groq API (Llama 3.3-70B), NLTK, Stanza |
| **OCR** | EasyOCR, PyMuPDF, python-docx, python-pptx |
| **Frontend** | Vanilla JS, Modern CSS (Glassmorphism, dark theme) |
| **Animation** | SIGML + CWA Signing Avatar Player |
| **GPU** | PyTorch (CUDA auto-detect) |
| **Deployment** | Gunicorn + Render |

---

## 🔌 API Reference

### `POST /translate`
Translate text to sign language gloss + SIGML animation sequence.

**Request body (JSON):**
```json
{
  "text": "Water is H2O",
  "language": "isl"
}
```

**Response:**
```json
{
  "gloss": ["WATER", "H", "TWO", "O"],
  "sigml_files": ["water", "H", "2", "O"],
  "concept_info": { "name": "...", "meaning": "..." }
}
```

### `POST /upload`
Upload a document or image to extract and translate its text.

**Form data:** `file` (PDF, DOCX, PPTX, PNG, JPG, etc.)

### `GET /history`
Returns the list of past translations stored in `history.json`.

---

## 🙏 Credits

- **SIGML Avatar Player**: [CWA Signing Avatars — University of East Anglia](https://vh.cmp.uea.ac.uk/index.php/CWA_Signing_Avatars)
- **LLM**: [Groq — Ultra-fast Llama 3.3 inference](https://groq.com)
- **Base ISL Vocabulary**: Open-source SIGML research corpus

---

## 📄 License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.
