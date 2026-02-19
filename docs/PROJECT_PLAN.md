# MedMentor AI — Project Development Plan

**Version:** 0.0.1 (Google ADK Agent-First Architecture)
**Date:** 2026-02-19
**Status:** Approved

This document outlines the step-by-step execution plan to build MedMentor AI (Full Production Release) as defined in `docs/PRD.md`. It aligns with the **Google ADK agent-first architecture** in `docs/ARCHITECTURE.md` and design system in `docs/DESIGN.md`. All features are mandatory for launch.

---

## 1. Executive Summary & Timeline

**Goal:** Launch Production Release in ~18-20 weeks.
**Focus:** **Agent-First Architecture** — All backend capabilities delivered through Google ADK agents.
**Critical Path:** Auth → ADK Agent Infrastructure → Core Agents → Learning Engine → Web Client.

### Key Architectural Shift
- **FROM:** Traditional API with AI features
- **TO:** AI Agent Orchestrator (Google ADK) coordinating all backend operations
- **A2A Protocol:** Agent-to-Agent communication for scalability and modularity
- **Agent Studio:** User-creatable agents in v0.1

### Phases
| Phase | Focus | Duration | Key Deliverables |
| :--- | :--- | :--- | :--- |
| **1** | **Foundation** | W1-2 | Repo, Infrastructure, DB, Auth, Design Tokens |
| **2** | **ADK Agent Infrastructure** | W3-4 | Google ADK setup, Supervisor Agent, Tool Layer, A2A setup |
| **3** | **Core Learning Agents** | W5-8 | Tutor, Retrieval, Safety, Quiz, Flashcard Agents |
| **4** | **Agent Studio** | W9-10 | Agent Builder UI, Templates, Testing, Sharing |
| **5** | **Tool Integration & A2A** | W11-12 | MCP (optional), A2A remote agents, Calendar, Search |
| **6** | **Web Client** | W13-15 | Dashboard, Study Interfaces, Library |
| **7** | **Mobile & Extension** | W16-18 | Mobile App, Browser Extension |
| **8** | **Polish & Launch** | W19-20 | Security Audit, Load Testing, Production Deploy |

---

## Phase 1: Foundation & Infrastructure (Weeks 1-2)

### 1.1 Repository & Monorepo Setup
- [ ] Initialize Git monorepo (Turborepo or Nx recommended).
    - `apps/web` (React/Vite)
    - `apps/mobile` (React Native/Expo)
    - `apps/extension` (Chrome/Edge)
    - `packages/ui` (Shared Design System)
    - `packages/api` (Backend/Node.js)
    - `packages/core` (Shared Logic/Types)
- [ ] Configure `eslint`, `prettier`, `typescript` (Strict Mode).
- [ ] Set up pre-commit hooks (Husky).

### 1.2 Infrastructure (VPS Setup)
- [ ] Provision **Hostinger VPS** (Ubuntu/Docker environment).
- [ ] Configure **Docker & Docker Compose** for service orchestration.
- [ ] Set up **PostgreSQL** (Self-hosted on VPS).
- [ ] Set up **Vector Database** (pgvector on VPS or self-hosted Milvus).
- [ ] Set up **Object Storage** (Cloudflare R2).
- [ ] Set up **Reverse Proxy** (Nginx/Traefik) with SSL (Let's Encrypt).
- [ ] Set up **CI/CD Pipelines** (GitHub Actions):
    - Lint/Test on PR.
    - Build & Deploy via SSH to VPS on merge to `main`.

### 1.3 Design System Implementation
- [ ] Create `packages/ui` library.
- [ ] Implement Design Tokens (`docs/DESIGN.md` §2, §3, §4).
    - Export as CSS Variables.
    - Tailwind config preset.
- [ ] Build Core Atoms from `docs/DESIGN.md`:
    - `Button`, `Card` (Glass/Light), `Input` (Min 44px touch targets).
    - `Typography` components (Fluid type scales).
    - `Icon` set (Lucide).
- [ ] **Mobile-First Verification**:
    - Verify all components on 320px viewport width.
    - Validate touch target sizing (WCAG 2.2 AA).

### 1.4 Authentication & User Schema
- [ ] Set up Auth Provider (Clerk, Auth0, or Supabase Auth).
    - Configure OAuth (Google, Apple, Microsoft).
- [ ] Define `User` schema in Postgres.
    - Fields: `email`, `preferences` (locale, timezone), `subscription_status`.

---

## Phase 2: ADK Agent Infrastructure (Weeks 3-4)

### 2.1 Google ADK Setup
- [ ] Install and configure **Google ADK** (Python or TypeScript)
- [ ] Set up **Vertex AI** integration with model router (LiteLLM for multi-provider)
- [ ] Configure **A2A protocol** for agent-to-agent communication
- [ ] Set up **ADK Tool Layer** (database, vector search, file storage tools)
- [ ] Implement **model router** with fallback logic

### 2.2 Supervisor Agent
- [ ] Implement **Supervisor Agent** (central orchestrator)
- [ ] Intent classification logic (Tutor, Quiz, Flashcard, Schedule, Search)
- [ ] Agent handoff coordination
- [ ] Session state management (ADK session state)
- [ ] Response aggregation from multiple agents

### 2.3 Safety Agent (Critical Path)
- [ ] Implement **Safety Agent** (all outputs route through this)
- [ ] Medical safety policy engine
- [ ] Refusal logic for patient-specific queries
- [ ] Citation verification and uncertainty labeling
- [ ] PII redaction tools

### 2.4 Tool Layer Implementation
- [ ] **Vertex AI Tool**: LLM model access with Gemini
- [ ] **Vector Search Tool**: RAG retrieval from user uploads
- [ ] **Citation Tool**: Source linking and verification
- [ ] **Database Tool**: CRUD operations for PostgreSQL
- [ ] **File Storage Tool**: Cloudflare R2 or Cloud Storage integration
- [ ] **Calendar Tool**: ICS export, Google Calendar integration

### 2.5 API Gateway (Lightweight)
- [ ] Set up **FastAPI** (Python) or **Express** (Node.js) gateway
- [ ] Authentication middleware (JWT, OAuth)
- [ ] Rate limiting (Redis-backed)
- [ ] Request routing to ADK agents
- [ ] Streaming response support
- [ ] Error handling and fallback

### 2.6 Database Schema
- [ ] Define PostgreSQL schema with pgvector
- [ ] Create tables for users, agents, conversations, citations
- [ ] **AgentConfig** table (for Agent Studio user-created agents)
- [ ] **AgentExecution** table (for observability)

---

## Phase 3: Core Learning Agents (Weeks 5-8)

### 3.1 Tutor Agent
- [ ] Implement **Tutor Agent** with Socratic method
- [ ] Layered explanation depths (Simple → Student → Clinical)
- [ ] Analogy and mnemonic generation
- [ ] Concept linking and cross-references
- [ ] Streaming response support

### 3.2 Retrieval/Citation Agent
- [ ] Implement **Retrieval Agent** for RAG
- [ ] Vector search across user uploads and knowledge library
- [ ] Citation extraction and verification
- [ ] Source ranking by authority
- [ ] Context window management

### 3.3 Quiz Agent
- [ ] Implement **Quiz Agent** with distractor rationales
- [ ] Multiple question types (SBA, multiple-correct, extended matching)
- [ ] Bloom's taxonomy alignment
- [ ] High-yield takeaway identification
- [ ] Template library integration

### 3.4 Flashcard Agent
- [ ] Implement **Flashcard Agent** with SRS data
- [ ] Card types: basic, cloze, image occlusion
- [ ] Deduplication logic
- [ ] Testability verification
- [ ] SRS parameter calculation (SM-2 or FSRS)

### 3.5 Scheduler Agent
- [ ] Implement **Scheduler Agent** for adaptive planning
- [ ] Task generation based on user goals
- [ ] Workload prediction and burnout prevention
- [ ] Auto-rescheduling logic
- [ ] Calendar integration (ICS, Google Calendar)

---

## Phase 4: Agent Studio (Weeks 9-10)

### 4.1 Agent Builder UI
- [ ] **YAML Mode**: Code-first agent definition interface
- [ ] **Visual Builder**: Drag-and-drop agent creation (optional for v0.1)
- [ ] Instruction builder with templates
- [ ] Tool selection interface
- [ ] Data source configuration

### 4.2 Agent Templates
- [ ] Built-in templates (Specialty Tutor, Quiz Generator, Flashcard Creator)
- [ ] Template cloning and customization
- [ ] Template versioning

### 4.3 Agent Testing & Validation
- [ ] Test chat interface for agent validation
- [ ] Safety test suite (intent tests, refusal tests)
- [ ] Performance validation (latency < 5 seconds)
- [ ] Deployment gates (safety checks, hallucination detection)

### 4.4 Agent Sharing & Marketplace
- [ ] Agent visibility settings (private, cohort, public)
- [ ] Agent publishing workflow
- [ ] Community agent browsing and search
- [ ] Agent installation (one-click add)
- [ ] Rating and review system
- [ ] Version control and rollback

---

## Phase 5: Tool Integration & A2A (Weeks 11-12)

### 5.1 MCP Integration (Optional)
- [ ] MCP server implementation (optional, recommended for extensibility)
- [ ] MCP tool discovery and registration
- [ ] Permission management for MCP tools
- [ ] MCP community tools integration

### 5.2 A2A Remote Agents
- [ ] **Analytics Agent** deployment (separate server)
- [ ] **Scheduler Agent** deployment (optional, separate server)
- [ ] A2A authentication and security
- [ ] Load balancing and health checks
- [ ] Circuit breaker implementation

### 5.3 Calendar Integration
- [ ] ICS export implementation
- [ ] Google Calendar two-way sync (write-focused)
- [ ] Privacy level controls (Private vs Detailed)

### 5.4 Search Integration
- [ ] Full-text search implementation
- [ ] Multi-language support
- [ ] Synonym and glossary integration

---

## Phase 6: Web Client Implementation (Weeks 13-15)
    - Auth Guard (JWT verify).
    - Logging (Start with stdout/Docker logs).
    - Rate Limiting (Redis-backed).
    - CORS.
- [ ] Define Swagger/OpenAPI spec.

### 2.2 User Profile & Settings
- [ ] Implement `GET /v1/me` and `PATCH /v1/me`.
- [ ] Implement Preferences Logic:
    - Language (I18n) storage.
    - Timezone detection/storage.

### 2.3 Internationalization (I18n) Foundation
- [ ] Set up `i18next` (or similar) in shared core.
- [ ] specific locale support (en, es, ar - RTL).
- [ ] Externalize strings for Error Messages and API Responses.

### 2.4 Billing (Scaffolding)
- [ ] Integrate Stripe (or equivalent).
- [ ] Define Products/Plans (Monthly/Yearly).
- [ ] Implement Webhook handler for subscription updates.
- [ ] **Nginx Config**: Ensure webhook endpoint is exposed securely.
- [ ] Database schema for `Subscription` and `Entitlement`.

---

## Phase 3: AI & RAG Pipeline (Weeks 5-7)

### 3.1 Upload & Ingestion Pipeline
- [ ] Implement `POST /v1/uploads`.
    - Validation (file type, size, quota).
    - Upload to **Cloudflare R2**.
- [ ] Create **Background Worker** (BullMQ + Redis on Docker):
    - Job: `process-document`.
    - Step 1: Text Extraction (OCR for images/PDFs).
    - Step 2: Chunking (Recursive character split).
    - Step 3: Metadata extraction (Title, Page numbers).

### 3.2 Vector Indexing
- [ ] Implement Embedding generation (OpenAI `text-embedding-3-small` or similar).
- [ ] Upsert chunks to Vector DB with metadata:
    - `user_id`, `doc_id`, `chunk_index`, `page_ref`.

### 3.3 AI Orchestrator (The Brain)
- [ ] Implement **Model Router**:
    - Select LLM based on task complexity/language (Prioritize strong medical reasoning models).
- [ ] Create **Tutor Agent** (LLM-First Strategy):
    - **Primary Source:** Use LLM's internal medical knowledge base.
    - **Optional Context:** Inject RAG chunks from user uploads *if available*.
    - System Prompt engineering (Persona, Tone, Socratic method).
    - Citation formatting logic (distinguish between general medical knowledge and user-provided notes).
- [ ] Create **Retrieval Agent** (Supplementary):
    - Logic: Query Rewriting -> Vector Search (Only if user context exists) -> Rerank.

### 3.4 Safety & Guardrails
- [ ] Implement PII Redaction on ingestion (optional but recommended).
- [ ] Output Guardrails:
    - Refusal check (Medical advice / Diagnosis).
    - "Uncited" warning logic.
    - Hallucination detection strategy (Self-check prompt).

---

## Phase 4: Learning Engine (Weeks 8-9)

### 4.1 Artifact Generation
- [ ] Implement `generate-flashcards` Job.
- [ ] Implement `generate-quiz` Job.
- [ ] Schema:
    - `Deck`, `Card` (Front/Back/Cloze).
    - `Quiz`, `Question`, `Distractor` (Rationales).

### 4.2 Spaced Repetition System (SRS)
- [ ] Implement Scheduling Algorithm (SM-2 or FSRS).
    - Inputs: `difficulty`, `prev_interval`.
    - Outputs: `next_review_date`.
- [ ] Endpoints:
    - `GET /v1/reviews/due`
    - `POST /v1/reviews/{cardId}/attempt` (updates schedule).

### 4.3 Mistake Notebook
- [ ] Logic: Identify "Missed" questions.
- [ ] Auto-create `MistakeEntry`.
- [ ] Trigger Remediation Agent (Generate explanation + follow-up card).

### 4.4 Study Scheduler
- [ ] Tasks Schema (`StudyTask`).
- [ ] ICS Export utility (`GET /v1/calendar/ics`).
- [ ] Logic for "Auto-reschedule overdue".

---

## Phase 5: Web Client Implementation (Weeks 10-11)

**Principle:** Mobile-first implementation. Build for small screens first, then enhance for desktop.

### 5.1 App Shell & Navigation
- [ ] mobile-first CSS architecture.
- [ ] Sidebar/Navbar (Responsive with Bottom Nav for mobile).
- [ ] Layouts (Dashboard, Focus Mode).
- [ ] Dark/Light Mode Toggles (Tailwind).

### 5.2 Core Workflows
- [ ] **Onboarding Flow**:
    - Goals, Exam details, Language selection.
- [ ] **Library View**:
    - Upload Drag-and-drop.
    - Document status indicators.
- [ ] **Tutor Chat Interface**:
    - Streaming responses.
    - Citation tooltips (Hover cards).

### 5.3 Study Interfaces
- [ ] **Flashcard Runner**:
    - Flip animation (CSS 3D).
    - SRS Feedback buttons (Again/Hard/Good/Easy).
- [ ] **Quiz Runner**:
    - Timer/Untimed modes.
    - Explanation reveal state.
    - "Flag" question feature.

### 5.4 Dashboard
- [ ] Streak display.
- [ ] Daily Due chart.
- [ ] Progress visualizers.

---

## Phase 6: Mobile & Extension (Weeks 15-17)

### 6.1 Mobile App (React Native/Expo) - Full Feature Parity
- [ ] Setup Navigation (Expo Router).
- [ ] Screen Parity (All Web Features):
    - Home (Dash).
    - Reviews (SRS Runner - Swipe gestures).
    - Chat (Tutor).
    - Library Management.
    - Settings & Profile.
- [ ] Offline Support (WatermelonDB or generic sync).
- [ ] Push Notifications (Expo Notifications).

### 6.2 Browser Extension
- [ ] Manifest v3 Setup.
- [ ] Content Script:
    - Highlight medical terms.
    - "Add to MedMentor" context menu.
- [ ] Popup:
    - Quick card add.
    - Mini Tutor chat.

---

## Phase 7: Optimization & Launch (Week 14)

### 7.1 Security & Compliance
- [ ] Rate Limiting audit.
- [ ] CORS lock-down.
- [ ] Data Export (GDPR) testing.
- [ ] Content Safety (Medical advice) Red-teaming.

### 7.2 Performance
- [ ] Database Indexing audit.
- [ ] Bundle size optimization (Lazy loading).
- [ ] Caching strategy (Redis for hot decks).

### 7.3 Analytics & Monitoring
- [ ] Set up Telemetry (PostHog - Cloud or Self-hosted).
- [ ] Server Monitoring (Container metrics / Uptime Kuma).
- [ ] AI Evals (Citation accuracy).

### 7.4 Final QA & Deployment
- [ ] E2E Testing (Playwright).
- [ ] Cross-browser / Cross-device testing.
- [ ] I18n verification (RTL layout check).
- [ ] **Production Deploy**:
    - Final `docker-compose.prod.yml` optimization.
    - Database backup/restore verification.
    - SSL Certificate auto-renewal check (Certbot).

---

## Dependencies & Risks

### Dependencies
- **LLM API Availability / Cost**: high volume of tokens for RAG.
- **OCR Accuracy**: Quality of optional uploaded PDFs directly impacts specific context retrieval (but not general knowledge).
- **Design System Fidelity**: Glassmorphism can be expensive on mobile; requires careful CSS implementation.

### Key Risks
- **Hallucinations**: AI inventing medical facts. Implementation of strict Grounding is P0.
- **Latency**: RAG pipeline can be slow. Needs aggressive caching and streaming.
- **Data Privacy**: Users uploading patient data. Needs clear disclaimers + Regex/NER scrubbing.
