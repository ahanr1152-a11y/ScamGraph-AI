# 🛡️ ScamGraph AI
### AI-Powered Scam Detection & Threat Intelligence Platform

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.0+-000000?style=flat&logo=flask&logoColor=white)](https://palletsprojects.com/p/flask/)
[![Supabase](https://img.shields.io/badge/Supabase-Cloud%20Database-3ECF8E?style=flat&logo=supabase&logoColor=white)](https://supabase.com/)
[![Vercel](https://img.shields.io/badge/Vercel-Deployment%20Ready-000000?style=flat&logo=vercel&logoColor=white)](https://vercel.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Detect suspicious messages. Connect scam syndicates. Visualize digital threats.**

ScamGraph AI is a web-based threat intelligence platform engineered to analyze suspicious communications, extract forensic indicators, and map multi-vector scam syndicates using interactive graph analytics. It pairs machine learning classification and rule-based heuristic scoring with OCR text extraction and cloud-backed threat relationship mapping.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [Graph Linking Logic](#-graph-linking-logic)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Installation and Setup](#-installation-and-setup)
- [Supabase Cloud Database Setup](#-supabase-cloud-database-setup)
- [Vercel Deployment](#-vercel-deployment)
- [Docker Deployment](#-docker-deployment)
- [Usage Workflow](#-usage-workflow)
- [Authors](#-authors)
- [License](#-license)

---

## 🎯 Overview

Digital scams frequently combine urgent psychological pressure, brand impersonation, deceptive URLs, and recycled contact endpoints across seemingly unrelated targets.

**ScamGraph AI** centralizes incident triage:
- Extracts threat indicators (phone numbers, URLs, UPI IDs, emails).
- Calculates comprehensive risk scores with transparent heuristic breakdowns.
- Constructs an interconnected threat graph highlighting coordinated scam syndicates sharing the same infrastructure.

---

## ✨ Key Features

- **🤖 Hybrid Threat Analysis Engine:**
  - **Machine Learning Classification:** Trained on spam/scam corpus using TF-IDF vectorization and Multinomial Naive Bayes.
  - **Heuristic Scoring System:** Multi-signal evaluation scoring credential phishing, urgent action pressure, fee requests, fake rewards, and corporate/bank impersonation.
- **🕸️ Interactive Vis.js Threat Graph:**
  - Real-time network topology visualizes shared indicators across independent reports.
  - Color-coded risk nodes: **Critical/High** (Red/Crimson), **Medium** (Amber), and **Low/Safe** (Green).
  - Physics-driven layout with node inspection modals and live refresh.
- **🖼️ OCR Screenshot Extraction:**
  - Extract text directly from suspicious SMS, WhatsApp, or email screenshots using Tesseract OCR with instant client-side preview.
- **☁️ Cloud & Local Storage Architecture:**
  - **Supabase Cloud (PostgreSQL via REST SDK):** Persistent storage across serverless and cloud instances.
  - **SQLite Fallback:** Automatic zero-config fallback for offline and local development.
- **🎨 Refined Modern Interface:**
  - Clean cybersecurity aesthetic with dark/light mode toggle.
  - Real-time service status dashboard monitoring API backend, OCR engine, and database connectivity.
  - Audit log with timestamps, risk indicators, and individual report deletion.
- **⚡ Serverless-Ready:** Native Vercel deployment configuration (`vercel.json` + `api/index.py`) and Docker containerization.

---

## ⚙️ How It Works

```
                     ┌───────────────────────────┐
                     │ Suspicious Text or Image  │
                     └─────────────┬─────────────┘
                                   │
                    [Image OCR / Direct Text Input]
                                   │
                                   ▼
        ┌─────────────────────────────────────────────────────┐
        │                 ScamGraph AI Engine                 │
        │  ├── Indicator Extraction (Phone, URL, UPI, Email)  │
        │  ├── Heuristic Rule Evaluation (Urgency, Imperson.) │
        │  └── Machine Learning Inference (scam_model.joblib) │
        └──────────────────────────┬──────────────────────────┘
                                   │
                         [Structured Output]
                                   │
                 ┌─────────────────┴─────────────────┐
                 ▼                                   ▼
      ┌─────────────────────┐             ┌─────────────────────┐
      │  Triage Risk Score  │             │   Graph Generator   │
      │  & Rule Explanations│             │  (Shared Indicators)│
      └──────────┬──────────┘             └──────────┬──────────┘
                 │                                   │
                 └─────────────────┬─────────────────┘
                                   │
                                   ▼
             ┌───────────────────────────────────────────┐
             │    Supabase Cloud DB / SQLite Storage     │
             └─────────────────────┬─────────────────────┘
                                   │
                                   ▼
             ┌───────────────────────────────────────────┐
             │       Interactive Web Investigation       │
             │           Dashboard (Vis.js UI)           │
             └───────────────────────────────────────────┘
```

1. **Intake:** The investigator inputs raw text or uploads an image (WhatsApp/SMS screenshot).
2. **Indicator Mining:** Regex and pattern extractors pull standardized identifiers:
   - Indian mobile numbers (e.g. `+91 98765 43210` → `9876543210`)
   - Fully qualified URLs and domain names
   - Virtual Payment Addresses / UPI IDs (`user@upi`, `id@okaxis`)
   - Email addresses
3. **Scoring:** The message is scored on a 0–100 risk scale:
   - 🔴 **High Risk (60–100):** Multiple critical indicators (e.g. credential harvest + urgent ultimatum + payment request).
   - 🟡 **Medium Risk (30–59):** Single threat indicators or suspicious prize notifications.
   - 🟢 **Low Risk (0–29):** Informational or routine messages without malicious intent patterns.
4. **Graph Construction:** Reports sharing identical indicator values are linked via edges, uncovering shared scam infrastructure.

---

## 🔗 Graph Linking Logic

The network graph dynamically builds relationships when reports share identical extracted indicators:

| Indicator Type | Extraction Pattern | Normalization |
|---|---|---|
| **Phone** | Indian 10-digit mobile (`6xxx-9xxx`) | Strips country code `+91` / spaces / hyphens |
| **URL** | HTTP / HTTPS / WWW endpoints | Case-insensitive matching |
| **UPI ID** | VPA addresses (e.g. `merchant@okhdfcbank`) | Lowercase normalization |
| **Email** | Standard email formats | Lowercase normalization |

> **Example:** If Report A reports a fake lottery mentioning `9876543210` and Report B reports a fake electricity bill mentioning `9876543210`, the graph immediately draws an edge connecting Report A and Report B.

---

## 🧰 Technology Stack

| Layer | Technology |
|---|---|
| **Backend Framework** | Python 3.10+, Flask |
| **Database** | Supabase REST SDK (`supabase-py`), SQLite3 |
| **Machine Learning** | Scikit-learn, Multinomial Naive Bayes, Joblib |
| **OCR Engine** | Tesseract OCR, Pytesseract, Pillow |
| **Graph Visualization** | Vis.js Network |
| **Frontend Styling** | Custom CSS3 (CSS Variables, Flexbox/Grid, Dark/Light mode) |
| **Deployment** | Vercel Serverless (`api/index.py`), Docker |

---

## 📁 Project Structure

```text
ScamGraph-AI/
├── api/
│   └── index.py             # Vercel serverless entry point
├── static/
│   ├── logo.png             # Application brand logo
│   └── style.css            # Responsive dashboard styles and dark/light themes
├── templates/
│   └── index.html           # Single-page investigation console with Vis.js
├── app.py                   # Main Flask application and REST routes
├── database.py              # Storage abstraction (Supabase cloud & SQLite fallback)
├── model.py                 # ML training script (UCI SMS Spam dataset)
├── scamgraph.py             # Feature extractor, scoring rules, and graph builder
├── scam_model.joblib        # Pre-trained ML model artifact
├── scamgraph.db             # Local SQLite database (for offline development)
├── requirements.txt         # Production Python dependencies
├── vercel.json              # Vercel deployment configuration
├── Dockerfile               # Container build configuration
├── .dockerignore
├── .gitignore
└── README.md
```

---

## 🚀 Installation and Setup

### Prerequisites

- Python 3.10 or higher
- Git
- Tesseract OCR engine (optional, required only for image OCR)

### 1. Clone the repository

```bash
git clone https://github.com/ahanr1152-a11y/ScamGraph-AI.git
cd ScamGraph-AI
```

### 2. Create a Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

*(Optional: To train the ML model from scratch, install `scikit-learn pandas` and run `python model.py`)*

### 4. (Optional) Install Tesseract OCR

- **Windows:** Download the installer from [UB-Mannheim Tesseract](https://github.com/UB-Mannheim/tesseract/wiki) and add it to your system `PATH`.
- **macOS:** `brew install tesseract`
- **Ubuntu/Debian:** `sudo apt-get install tesseract-ocr`

### 5. Launch the Dashboard

```bash
python app.py
```

Access the interface in your browser:
```text
http://127.0.0.1:5000
```

---

## ☁️ Supabase Cloud Database Setup

For persistent cloud storage (especially when deployed to Vercel or cloud runtimes):

1. Create a free project on [Supabase](https://supabase.com/).
2. Navigate to **SQL Editor** and execute the following schema:

```sql
CREATE TABLE IF NOT EXISTS reports (
    id              BIGSERIAL PRIMARY KEY,
    title           TEXT NOT NULL,
    source          TEXT NOT NULL,
    raw_text        TEXT NOT NULL,
    risk_score      INTEGER NOT NULL,
    risk_level      TEXT NOT NULL,
    indicators_json TEXT NOT NULL DEFAULT '[]',
    reasons_json    TEXT NOT NULL DEFAULT '[]',
    created_at      TEXT NOT NULL
);

-- Enable Row Level Security (RLS)
ALTER TABLE reports ENABLE ROW LEVEL SECURITY;

-- Allow anonymous access for the triage console
CREATE POLICY "Allow all for anon"
ON reports
FOR ALL
TO anon
USING (true)
WITH CHECK (true);
```

3. Copy your project credentials from **Project Settings → API**:
   - `Project URL`
   - `anon / public` API key
4. Set the environment variables locally or in your deployment platform:
   - `SUPABASE_URL="https://your-project.supabase.co"`
   - `SUPABASE_KEY="your-anon-public-key"`

---

## 🌐 Vercel Deployment

ScamGraph AI includes built-in Vercel serverless configuration:

1. Import the repository into your **[Vercel Dashboard](https://vercel.com/)**.
2. Under **Project Settings → Environment Variables**, add:
   - `SUPABASE_URL` = `https://your-project.supabase.co`
   - `SUPABASE_KEY` = `your-supabase-anon-key`
3. Click **Deploy**. The application runs seamlessly via `api/index.py`.

---

## 🐳 Docker Deployment

To build and run the application using Docker:

```bash
docker build -t scamgraph-ai .
docker run -p 5000:5000 -e SUPABASE_URL="your_url" -e SUPABASE_KEY="your_key" scamgraph-ai
```

---

## 🧪 Usage Workflow

1. **Submit Incident:** Enter an incident title and paste the suspicious text or upload a screenshot.
2. **Review Triage Assessment:** Check the computed risk score, risk level badge, and detailed rule justifications.
3. **Explore Relationships:** Inspect the **Interactive Threat Graph** to see if the newly reported incident shares any phone numbers, URLs, or payment handles with past reports.
4. **Audit History:** Browse past reports in the sidebar log, view indicator chips, or delete resolved entries.

---

## 👨‍💻 Authors

**Team TechSolvers**  
B.Tech CSE (AI & ML) — Puran Murti Vidyapeeth

- [@ahanr1152-a11y](https://github.com/ahanr1152-a11y)
- [@KrishKumar3k](https://github.com/KrishKumar3k)
- [@nilendunathjha14339-art](https://github.com/nilendunathjha14339-art)
- [@divyamdotlite](https://github.com/divyamdotlite)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
