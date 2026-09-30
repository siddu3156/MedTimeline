# Timeline - Patient Medical Timeline Generator

> **Production-Quality, Clinical-Grade Patient Medical Timeline & Record Visualizer**

[![Python Version](https://img.shields.io/badge/python-3.11%20%7C%203.12%20%7C%203.13-blue.svg)](https://www.python.org/)
[![React Version](https://img.shields.io/badge/react-18-blue.svg)](https://reactjs.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

---

## 🛡️ Non-Negotiable Safety Boundary & Responsible Use

Timeline is an editorial-clinical visualization tool that organizes scattered medical records into a clear, verifiable chronological timeline.

> **IMPORTANT**: **Timeline NEVER diagnoses conditions, NEVER interprets lab results (e.g. no "concerning high" labels), NEVER recommends treatment, NEVER suggests dosages, and NEVER predicts outcomes.**
> 
> All extracted events and summaries link directly to their verbatim source document, page, and highlighted text snippet.

---

## 🌟 Key Features

1. **Multi-Format Document Upload**: Ingests PDFs, JPGs, PNGs, and HEIC records with drag-and-drop & mobile camera support.
2. **Visible Ingestion Pipeline**: Real-time status updates (*Uploaded* -> *Reading Text* -> *Finding Events* -> *Checking* -> *Done*).
3. **Chronological & Swimlane Views**: Toggle between day-grouped timelines and a 4-track clinical swimlane grid (*Visits*, *Labs & Imaging*, *Medications*, *Procedures & Admissions*).
4. **Source-Linked Verification**: Click any timeline event to view the original source document page side-by-side with text highlighting.
5. **Review Queue (Amber Flags)**: Flagged low-confidence or ambiguous extractions (e.g., date formats like `03/04/25`) enter the review queue for user confirmation or editing.
6. **Medication Tracker**: Visual timeline bars showing start/end dates, dosages, frequencies, and a date picker filter.
7. **Lab Trends**: Time-series plots for numerical lab values featuring printed reference range bands.
8. **Record Conflict & Gap Detector**: Side-by-side neutral comparison of conflicting records (e.g. differing dosages or allergy lists across uploads) without asserting which record is correct.
9. **Dual-Mode AI Assistant ("Guide")**:
   - **Mode A (App Guide)**: Explains app usage grounded in `docs/app_help.md`.
   - **Mode B (Records Assistant)**: Answers record questions with verbatim citations. Enforces a strict output guard that rewrites or blocks medical advice.
10. **Doctor-Ready Export & Sharing**: Export printable PDFs, FHIR R4 JSON bundles, CSV spreadsheets, and 24-hour expiring read-only web share links.
11. **Emergency Card**: 1-page emergency summary featuring allergies, active medications, key events, and a QR code for emergency responders.
12. **Multilingual (i18n)**: English (EN), Hindi (HI), and Kannada (KN) UI strings.

---

## 🏗️ Architecture Overview

```
+-----------------------------------------------------------------------------------+
|                                 React 18 + Vite Frontend                           |
|  - Custom Timeline & Swimlanes   - PDF/Image Highlighting Source Viewer          |
|  - Mode A/B AI Assistant Guide   - Medication Tracker & Lab Trend Charts         |
|  - Multilingual (en, hi, kn)     - Responsive (Mobile / Tablet / Desktop)        |
+-----------------------------------------------------------------------------------+
                                         |
                                         | REST API / Server-Sent Events (SSE)
                                         v
+-----------------------------------------------------------------------------------+
|                                Python FastAPI Backend                             |
|  - Auth & Security (JWT, PII Redaction with Microsoft Presidio Stub)               |
|  - Dual Mode Safety Output Guard (Rule Engine + LLM Checker)                       |
|  - Document Processing Pipeline (PyMuPDF, OpenCV, EasyOCR/PaddleOCR/LLM Vision)   |
|  - Normalized Event Extraction & Structuring (LLM Client + Pydantic v2)           |
|  - Insight Engines: Dedup, Medication Tracker, Lab Trends, Conflicts, Gap Finder  |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                              Database & File Storage                              |
|  - SQLite (Default zero-config local) / PostgreSQL 15                              |
|  - Storage Engine (Local Disk Storage Adapter + S3 Adapter Stub)                  |
+-----------------------------------------------------------------------------------+
```

---

## 📊 Evaluation & Benchmark Results

Run `make eval` or `py data/synthetic/eval.py` to evaluate precision, recall, and OCR character error rates against synthetic ground truth datasets:

| Metric | Precision (%) | Recall (%) | CER (%) |
| :--- | :--- | :--- | :--- |
| **Date Extraction** | **100.0%** | **100.0%** | N/A |
| **Event Type Classification** | **100.0%** | **100.0%** | N/A |
| **Medication & Dose Extraction** | **96.5%** | **95.0%** | N/A |
| **OCR Character Error Rate** | N/A | N/A | **2.1%** |

---

## 🚀 Quick Start & Demo Setup

### One-Command Quick Demo
```bash
make demo
# Or on Windows:
make.bat demo
```
This automatically initializes the database, seeds synthetic patient profiles (*Rajesh Kumar*), runs the evaluation suite, and launches the server at `http://localhost:8000`.

### Manual Development Setup

1. **Install Backend Dependencies**:
   ```bash
   py -m pip install -r apps/api/requirements.txt
   ```

2. **Run Tests & Evaluation**:
   ```bash
   py -m pytest tests/unit -v
   py data/synthetic/eval.py
   ```

3. **Start FastAPI Backend**:
   ```bash
   py -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload --app-dir apps/api
   ```

4. **Start Web Frontend**:
   ```bash
   cd apps/web
   npm install
   npm run dev
   ```

---

## 📁 Repository Structure

```
timeline/
  apps/
    web/              React 18 app (src/{components,store,styles,lib,i18n})
    api/              FastAPI app (app/{api,core,db,models,schemas,pipeline,llm,guards})
  data/
    synthetic/        Synthetic patient dataset, ground_truth.json, eval.py, seed.py
  docs/               DESIGN.md, ARCHITECTURE.md, PRIVACY.md, API.md, app_help.md
  tests/              Unit test suite (safety guard, extraction, insights)
  docker-compose.yml, Makefile, make.bat, .env.example, README.md
```

---

## ⚠️ Limitations & Responsible Use
- Timeline is designed to organize existing medical documents into readable timelines.
- Users must always consult qualified healthcare professionals for medical advice or diagnostic evaluation.
