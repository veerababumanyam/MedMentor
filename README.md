# 🩺 MedMentor AI - The Interconnected Medical Brain

[![Project Status: Alpha](https://img.shields.io/badge/Status-Alpha-orange.svg)](#)
[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red.svg)](#)
[![Framework: Next.js](https://img.shields.io/badge/Framework-Next.js-blue.svg)](#)
[![AI: Google ADK](https://img.shields.io/badge/AI-Google_ADK-green.svg)](#)

> **Vision:** Turning medical "information overload" into a trustworthy, evidence-grounded, and personalized learning experience.

MedMentor AI is a next-generation medical education platform that transforms dense curricula into adaptive study plans. Built with an **agent-first architecture**, it connects every piece of information—from lecture PDFs to clinical cases—into a unified, intelligent knowledge network.

---

## ✨ Core Pillars

### 1. The Interconnected Brain
MedMentor isn't just a document storage tool. It's a system where every upload (PDF, DOCX, Image) is analyzed and cross-linked. A concept mentioned in a lecture becomes a flashcard, which links to a quiz, which links back to a source-cited explanation.

### 2. Agent-First Architecture
Powered by **Google Agent Development Kit (ADK)** and the **Agent-to-Agent (A2A) protocol**, MedMentor uses a swarm of specialized AI agents:
- **Supervisor:** Orchestrates requests and manages agent handoffs.
- **Tutor:** Provides layered, Socratic explanations (Simple → Student → Clinical).
- **Retrieval (RAG):** Grounds every claim in trustworthy evidence with non-fabricated citations.
- **Safety:** Enforces medical guidelines and patient safety (Not a diagnostic tool).

### 3. Precision Design
Implementing **Liquid Glassmorphism**, MedMentor provides an immersive, "iOS-grade" experience. The UI is designed to feel alive, using deep backdrop blurs, animated mesh backgrounds, and 4px grid precision to reduce cognitive load during intense study sessions.

---

## 🚀 Key Features

- **🧠 AI Tutor Chat:** Real-time deeply grounded medical tutoring with depth-on-demand.
- **📝 Automated Asset Generation:** Instantly turn your notes into summaries, hierarchical outlines, and mind maps.
- **🃏 Smart Flashcards (SRS):** Native spaced-repetition system supporting Cloze, Image Occlusion, and Anki-compatible exports.
- **📊 Adaptive Quizzes:** Topic-based quiz builder with exhaustive distractor rationales—understand *why* an answer is wrong.
- **📓 Mistake Notebook:** Automatically tracks missed questions and generates targeted remediation packages.
- **📅 Smart Scheduler:** A calendar-driven study plan that auto-reschedules when life happens, with burnout protection built-in.
- **🧪 Agent Studio:** Create, test, and share custom study agents tailored to specific medical specialties or exams.

---

## 🏗️ Technical Stack

- **Frontend:** Next.js (Web), Cross-platform Mobile (iOS/Android), Chrome/Edge Extension.
- **AI Core:** Google ADK, Vertex AI (Gemini), LiteLLM (Fallback Router).
- **Backend:** Python (FastAPI) / TypeScript (Express) with A2A Protocol for distributed agents.
- **Database:** PostgreSQL with `pgvector` for semantic search and RAG.
- **Storage:** Cloudflare R2 / Google Cloud Storage for medical assets.
- **Auth:** OAuth 2.0 (Google, Apple, Microsoft) with cross-device session syncing.

---

## 🛠️ Getting Started

### Prerequisites
- Node.js 18+
- Python 3.10+ (for agent backend)
- PostgreSQL with `pgvector`
- Google Cloud Project with Vertex AI enabled

### Installation (Dev Environment)
1. **Clone the repository:**
   ```bash
   git clone https://github.com/veerababumanyam/MedMentor.git
   cd MedMentor
   ```

2. **Setup Frontend:**
   ```bash
   npm install
   npm run dev
   ```

3. **Setup Agent Backend:**
   ```bash
   cd backend
   pip install -r requirements.txt
   python main.py
   ```

*(Detailed setup instructions can be found in `docs/ADK_SETUP.md`)*

---

## 🗺️ Roadmap

- **v0.1:** Core Learning Loop (Tutor, Quiz, Cards, Scheduler).
- **v0.2:** Browser Extension & Community Hub (Collaborative Decks).
- **v0.3:** Voice-based Virtual Patient & Clinical "Grill Mode" simulation.

---

## 🛡️ Trust, Safety & Compliance

MedMentor AI is designed as a **strictly educational tool**.
- **Not Medical Advice:** The system refuses to provide patient-specific diagnosis or treatment.
- **Evidence Labeling:** Qualitative confidence levels (High/Medium/Low) provided for all AI claims.
- **Privacy by Design:** HIPAA-aligned data handling and encryption at rest/transit.

---

## 📄 License

Proprietary. All rights reserved by **Veera Babu Manyam**.

---
*Developed with ❤️ for medical learners worldwide.*
