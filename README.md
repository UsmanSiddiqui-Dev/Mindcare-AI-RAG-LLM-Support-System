# MindCare — AI-Powered Mental Health Support System

A full-stack, safety-first mental health platform combining **RAG**, **LLM generative AI**, and **ML-based crisis detection** to deliver empathetic, context-aware support — with journaling, mood tracking, and a professional admin dashboard.

---

## Table of Contents

1. [Overview](#overview)
2. [Key Features](#key-features)
3. [System Architecture](#system-architecture)
4. [Tech Stack](#tech-stack)
5. [Project Structure](#project-structure)
6. [Getting Started](#getting-started)
7. [Environment Variables](#environment-variables)
8. [Usage](#usage)
9. [AI & ML Components](#ai--ml-components)
10. [Security & Compliance](#security--compliance)
11. [Deployment](#deployment)
12. [Screenshots](#screenshots)
13. [Disclaimer](#disclaimer)

---

## Overview

MindCare is a production-ready mental wellness platform that uses a multi-layered AI pipeline to identify and respond to mental health risks in real time. It combines a **Machine Learning Classification Pipeline (MCP)** for crisis detection, **Retrieval-Augmented Generation (RAG)** for knowledge-enriched responses, and **large language models** (GPT-4, Claude) for empathetic conversation — all behind a secure FastAPI backend with JWT authentication.

Beyond crisis response, MindCare provides users with daily mood tracking, private journaling with sentiment analysis, and visualized emotional trends — giving individuals a holistic tool for ongoing mental wellness.

---

## Key Features

- **Real-Time Crisis Detection**  
  TF-IDF + Logistic Regression classifier flags high-risk content before it reaches the LLM, triggering immediate crisis resource delivery.

- **RAG-Enhanced Responses**  
  Curated mental health knowledge bases are retrieved and injected into LLM context, ensuring responses are grounded in evidence-based guidance.

- **Generative AI Chat**  
  Powered by GitHub Models API (GPT-4, Claude, o1) — delivers empathetic, context-aware conversations at scale.

- **Mood & Journal Tracking**  
  Daily mood logging with emoji/numeric scales, private journaling with optional sentiment analysis, and trend charts for visualizing emotional patterns over time.

- **Professional Admin Dashboard**  
  Secure interface for mental health professionals to review flagged messages, assess risk levels, escalate urgent cases, and access anonymized mood/journal summaries (with user consent).

- **JWT Authentication**  
  Secure user registration and login. Role-based access separates standard users from admin-level professionals.

- **Audit Logging**  
  Full event logging to support GDPR and HIPAA-aligned compliance requirements.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     React Frontend                       │
│        (TypeScript · Tailwind CSS · Framer Motion)       │
└───────────────────────┬─────────────────────────────────┘
                        │ REST API (JWT)
┌───────────────────────▼─────────────────────────────────┐
│                  FastAPI Backend                          │
│         User auth · API routing · Audit logging          │
└──────┬────────────────┬────────────────────┬────────────┘
       │                │                    │
┌──────▼──────┐ ┌───────▼──────┐ ┌──────────▼──────────┐
│  MCP Module │ │  RAG Module  │ │  GitHub Models API   │
│  TF-IDF +   │ │  Knowledge   │ │  GPT-4 · Claude · o1 │
│  LogReg     │ │  Retrieval   │ │  (Generative LLM)    │
└─────────────┘ └──────────────┘ └─────────────────────-┘
       │
┌──────▼──────────────────────────────────────────────────┐
│             SQLite / PostgreSQL Database                  │
│        Users · Conversations · Mood · Journals           │
└─────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, TypeScript, Tailwind CSS, Framer Motion, Vite |
| **Backend** | Python, FastAPI, Uvicorn |
| **Database** | SQLite (dev) / PostgreSQL (prod) |
| **ML / NLP** | Scikit-learn (TF-IDF + Logistic Regression), Joblib |
| **RAG** | Custom retrieval pipeline with curated mental health corpus |
| **LLM** | GitHub Models API — GPT-4, Claude, o1 |
| **Auth** | JWT (JSON Web Tokens) |
| **Deployment** | Docker, Vercel (frontend), Render (backend) |

---

## Project Structure

```
mindcare-ai-rag-llm-support-system/
├── src/                        # React frontend
│   ├── components/             # Reusable UI components
│   │   ├── chat/               # Chat message + input
│   │   ├── layout/             # Navbar, Sidebar, Layout
│   │   ├── mood/               # Mood selector + chart
│   │   └── ui/                 # Button, Input, TextArea
│   ├── pages/                  # Route-level page components
│   ├── context/                # Auth context (JWT state)
│   ├── utils/                  # API calls, storage, chat logic
│   └── types/                  # TypeScript type definitions
├── backend/
│   ├── main.py                 # FastAPI app — routes, auth, DB
│   └── requirements.txt        # Python dependencies
├── MCP/
│   ├── mcpapi.py               # ML classification API
│   ├── logreg_model.joblib     # Trained logistic regression model
│   └── vectorizer.joblib       # TF-IDF vectorizer
├── rag/
│   ├── enhanced_suicide_detection_api.py
│   └── simplified_suicide_detection_api.py
├── .env.example                # Environment variable template
├── .env.local.example          # Local frontend env template
├── vercel.json                 # Vercel deployment config
├── docker-compose.yml          # (optional) Container orchestration
└── README.md
```

---

## Getting Started

### Prerequisites

- Node.js 18+
- Python 3.10+
- Git

### 1. Clone the Repository

```bash
git clone https://github.com/UsmanSiddiqui-Dev/mindcare-ai-rag-llm-support-system.git
cd mindcare-ai-rag-llm-support-system
```

### 2. Frontend Setup

```bash
npm install
cp .env.local.example .env.local
# Add your VITE_API_BASE_URL in .env.local
npm run dev
```

### 3. Backend Setup

```bash
cd backend
pip install -r requirements.txt
cp .env.example .env
# Add your secrets in .env
uvicorn main:app --reload --port 8000
```

### 4. Start ML & RAG APIs

```bash
# From project root
bash start_apis.sh
```

---

## Environment Variables

### Frontend (`.env.local`)

| Variable | Description |
|---|---|
| `VITE_API_BASE_URL` | Backend API base URL |
| `VITE_GITHUB_TOKEN` | GitHub Models API token for LLM access |

### Backend (`.env`)

| Variable | Description |
|---|---|
| `SECRET_KEY` | JWT signing secret |
| `DATABASE_URL` | SQLite or PostgreSQL connection string |
| `FRONTEND_ORIGINS` | Allowed CORS origins |
| `APP_NAME` | Application name |

See `.env.example` and `.env.local.example` for full reference.

---

## Usage

| Route | Description |
|---|---|
| `/` | Home — daily mood check-in + motivational quote |
| `/chat` | AI therapy chat with crisis detection |
| `/journal` | Private journal with sentiment analysis |
| `/mood` | Mood trend charts and history |
| `/profile` | User profile management |
| `/admin` | Admin dashboard (role-gated) |

---

## AI & ML Components

### Machine Learning Classification Pipeline (MCP)

- **Preprocessing:** Text cleaning, tokenization, stop-word removal  
- **Feature extraction:** TF-IDF vectorization  
- **Classifier:** Logistic Regression (trained on mental health datasets)  
- **Keyword matching:** Secondary real-time filter for self-harm language  
- Pre-trained models stored as `logreg_model.joblib` and `vectorizer.joblib`

### RAG Pipeline

- Domain-specific mental health corpus ingested and indexed at startup  
- On each user message, relevant passages are retrieved and injected into the LLM prompt  
- Grounds generative responses in evidence-based mental health guidance  

### LLM Integration

- **Provider:** GitHub Models API  
- **Models available:** GPT-4, Claude, o1  
- Automatic escalation logic: high-risk messages bypass standard LLM response and deliver crisis resources directly

---

## Security & Compliance

- **JWT authentication** — tokens are short-lived and refreshed server-side  
- **Server-side API keys** — no LLM credentials exposed to the client  
- **Audit logging** — every flagged event is recorded for compliance review  
- **Role-based access** — admin routes are protected at the API layer  
- **Privacy-safe design** — no third-party data sharing; local-first storage option available  

---

## Deployment

### Frontend — Vercel

```bash
# Push to GitHub, then import repo in Vercel dashboard
# Set environment variables in Vercel project settings
```

### Backend — Render / Docker

```bash
docker-compose up --build
```

See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) and [DEPLOYMENT_STEPS.md](DEPLOYMENT_STEPS.md) for full production setup.

---

## Screenshots

![Home](https://github.com/user-attachments/assets/55cffe50-f661-4573-b520-b2f82919459e)
![Chat](https://github.com/user-attachments/assets/8a9b6c7a-45ca-4438-9c95-ce3680205a97)
![Mood Tracker](https://github.com/user-attachments/assets/826efad9-a55c-4319-8aab-a3d2c334066b)
![Admin Dashboard](https://github.com/user-attachments/assets/f42f2c1f-a7dc-4091-8373-6966045163b6)

---

## Disclaimer

MindCare is **not a substitute for professional mental health care**. If you or someone you know is in crisis, please contact a licensed mental health professional or a crisis helpline immediately.

- **International Association for Suicide Prevention:** https://www.iasp.info/resources/Crisis_Centres/
- **Crisis Text Line (US):** Text HOME to 741741
