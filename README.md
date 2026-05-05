# 🔬 Ingredient IQ — Smart Ingredient Analyzer

> Upload a product label image or paste an ingredient list to instantly get AI-powered safety analysis, harm scores, and risk classifications.

**🌐 Live Demo:** [smart-incrident-analyzer-eypz.vercel.app](https://smart-incrident-analyzer-eypz.vercel.app)

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [API Documentation](#-api-documentation)
- [How It Works](#-how-it-works)
- [ML Model](#-ml-model)
- [Future Scope](#-future-scope)
- [License](#-license)

---

## ✨ Features

### For Users
- **📸 Image Upload** — Drag-and-drop product label images
- **🔍 OCR Extraction** — Automatic text extraction via Tesseract.js (client-side, no server needed)
- **✍️ Manual Input** — Paste or type ingredient lists directly
- **📊 Safety Dashboard** — Comprehensive visual results including:
  - Overall safety score (1–10 scale)
  - Risk classification: ✅ Safe / ⚠️ Moderate Risk / ❌ Harmful
  - Ingredient breakdown by category with expandable detail cards
  - Interactive doughnut chart visualization
- **📱 Responsive Design** — Works seamlessly on desktop and mobile

### For Developers
- **FastAPI Backend** — High-performance REST API with auto-generated Swagger docs
- **TensorFlow LSTM** — ML model for predicting harm scores on unknown ingredients
- **CORS Enabled** — Ready for any frontend integration
- **Health Check Endpoint** — Built-in monitoring at `/health`

---

## 🛠️ Tech Stack

### Frontend

| Technology | Purpose |
|---|---|
| React 18 + TypeScript | UI framework |
| Vite | Build tool & dev server |
| Tailwind CSS | Styling |
| shadcn/ui + Radix UI | Component library |
| React Query | Data fetching & caching |
| React Router DOM | Client-side routing |
| Tesseract.js | Client-side OCR |
| Chart.js + react-chartjs-2 | Data visualization |
| Lucide React | Icon set |

### Backend

| Technology | Purpose |
|---|---|
| FastAPI | Python web framework |
| Uvicorn | ASGI server |
| TensorFlow 2.16.2 | LSTM model inference |
| PyTesseract | Server-side OCR |
| Pillow | Image processing |
| Pydantic | Data validation |

### Machine Learning
- **LSTM Neural Network** — Predicts harm scores for unknown ingredients
- **Tokenizer** — Text preprocessing for model input
- **Training Dataset** — Curated database of 229 known ingredients

---

## 📁 Project Structure

```
Smart-Incrident-Analyzer/
│
├── Backend/
│   ├── main.py                  # FastAPI application
│   ├── requirements.txt         # Python dependencies
│   ├── runtime.txt              # Python 3.11.0 specification
│   ├── lstm_model.keras         # Trained LSTM model (3.4 MB)
│   └── tokenizer.pkl            # Fitted tokenizer
│
└── Frontend/
    ├── package.json
    ├── vite.config.ts           # Dev server on port 8080
    ├── tsconfig.json
    ├── index.html
    └── src/
        ├── main.tsx
        ├── App.tsx
        ├── components/
        │   ├── ui/              # shadcn/ui components (28+)
        │   ├── Header.tsx
        │   ├── Footer.tsx
        │   ├── Hero.tsx
        │   ├── ImageUpload.tsx
        │   ├── ExtractedIngredients.tsx
        │   ├── AnalysisResults.tsx
        │   └── NavLink.tsx
        ├── pages/
        │   ├── Index.tsx
        │   └── NotFound.tsx
        ├── hooks/
        │   └── use-mobile.tsx
        └── lib/
            └── utils.ts
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v18 or higher
- **Python** 3.11
- **Git**

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/Smart-Incrident-Analyzer.git
cd Smart-Incrident-Analyzer
```

### 2. Set Up the Backend

```bash
cd Backend

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt

# Start the FastAPI server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

- API running at: `http://localhost:8000`
- Interactive docs: `http://localhost:8000/docs`

### 3. Set Up the Frontend

```bash
cd Frontend

npm install
npm run dev
```

- App running at: `http://localhost:8080`

### 4. (Optional) Configure API URL

Create a `.env` file in the `Frontend/` directory:

```env
VITE_API_URL=http://localhost:8000
```

> By default the frontend points to the deployed Render backend.

---

## 📡 API Documentation

**Production base URL:**
```
https://smart-incrident-analyzer-full-project.onrender.com
```

### `POST /analyze`
Analyzes a comma-separated ingredient list.

**Request body:**
```json
{
  "ingredients": "water, glycerin, niacinamide, hyaluronic acid"
}
```

**Response:**
```json
{
  "ingredients": [
    {
      "name": "glycerin",
      "score": 1.0,
      "category": "safe",
      "description": "Humectant that draws water into the skin"
    }
  ],
  "average_score": 3.2,
  "max_score": 7.0,
  "final_score": 5.1,
  "classification": "Moderate Risk"
}
```

---

### `POST /ocr`
Extracts text from an uploaded image.

**Request:** `multipart/form-data` with a `file` field (image)

**Response:**
```json
{
  "text": "extracted ingredient text from image"
}
```

---

### `GET /health`
Health check endpoint.

**Response:**
```json
{
  "status": "healthy"
}
```

---

## 🔬 How It Works

### Analysis Pipeline

```
User Input (Image / Text)
        │
        ▼
   OCR Extraction (Tesseract.js)
        │
        ▼
  Text Normalization (lowercase, trim)
        │
        ▼
  Database Lookup (229 known ingredients)
        │
   ┌────┴────────────────┐
Known?                Unknown?
   │                      │
   ▼                      ▼
Retrieve           LSTM ML Prediction
harm_score            harm_score
        │
        ▼
   Score Calculation
   ┌─────────────────────────────────────────┐
   │ final_score = (avg_score + max_score)/2 │
   └─────────────────────────────────────────┘
        │
        ▼
   Risk Classification
   ✅ 1.0 – 5.0  → Safe
   ⚠️ 5.1 – 7.0  → Moderate Risk
   ❌ 7.1 – 10.0 → Harmful
```

---

## 🤖 ML Model

| Property | Detail |
|---|---|
| Architecture | LSTM (Long Short-Term Memory) |
| Framework | TensorFlow 2.16.2 (Keras API) |
| Input | Ingredient name (tokenized) |
| Output | Harm score (1–10) |
| Model size | 3.4 MB |

The model generalizes across chemical naming patterns, enabling predictions for thousands of ingredients not present in the known-ingredient database.

**Inference flow:**
```
Ingredient Name → Tokenization → LSTM Model → Harm Score (1–10)
```

---

## 🎯 Future Scope

- [ ] User accounts with saved analysis history
- [ ] Pre-analyzed common product database
- [ ] Batch analysis for multiple products
- [ ] Third-party API key integration
- [ ] Native iOS & Android app
- [ ] Multilingual OCR and analysis support
- [ ] Ingredient interaction detection (flag harmful combos)
- [ ] Personalized safer-alternative recommendations
- [ ] Live camera real-time scanning
- [ ] Community-sourced ingredient ratings

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [shadcn/ui](https://ui.shadcn.com/) — Accessible, beautiful UI components
- [Tesseract.js](https://tesseract.projectnaptha.com/) — Client-side OCR engine
- [Chart.js](https://www.chartjs.org/) — Flexible charting library
- [Lucide](https://lucide.dev/) — Clean, consistent icon set

---

## 📬 Contact

**Project Maintainer:** [Your Name]
**GitHub:** [@yourusername](https://github.com/yourusername)
**Email:** your.email@example.com

---

*Built with ❤️ and machine learning*
