# MedMentor AI — Project Development Plan

**Version:** 0.0.1 (Google ADK Agent-First Architecture)
**Date:** 2026-02-19
**Status:** Approved

This document outlines the step-by-step execution plan to build MedMentor AI (Full Production Release) as defined in `docs/PRD.md`. It aligns with the **Google ADK agent-first architecture** in `docs/ARCHITECTURE.md` and design system in `docs/DESIGN.md`. All features are mandatory for launch.

---

## 1. Executive Summary & Timeline

**Goal:** Launch Production Release in ~18-20 weeks.
**Focus:** **Research-First AI Agents** — Empowering learners through deep research specialists integrated with a chat-first UI.
**Critical Path:** Auth → AI Research Agent Infrastructure → Core Search Agents → Learning Engine → Web Client.

### Key Architectural Shift
- **FROM:** Traditional API with AI features
- **TO:** Integrated AI Research Agents coordinating all tasks through deep search and synthesis.
- **A2A Protocol:** Agent-to-Agent communication for scalability and modularity
- **Agent Studio:** User-creatable agents in v0.1

### Phases
| Phase | Focus | Duration | Key Deliverables |
| :--- | :--- | :--- | :--- |
| **1** | **Foundation** | W1-2 | Repo, Infrastructure, DB, Auth, Design Tokens |
| **2** | **AI Agent Infrastructure & Search** | W3-4 | Google ADK, Supervisor, Internet Search & Deep Research Tools |
| **3** | **Core Research Agents** | W5-8 | Tutor (Search-linked), Retrieval (Web-First), Safety, Quiz, Flashcard |
| **4** | **Chat-First Web Client** | W9-11 | Integrated Chat UI, Research Drawer, Dashboards |
| **5** | **Document Ingestion (Additional)** | W12-13 | OCR/Parsing, Vector Search from user documents, personalization |
| **6** | **Agent Studio & A2A** | W14-16 | Agent Builder UI, Sharing, A2A Remote Agents |
| **7** | **Mobile & Extension** | W17-18 | Mobile App, Browser Extension |
| **8** | **Polish & Launch** | W19-20 | Security Audit, Load Testing, Production Deploy |

---

## Phase 1: Foundation & Infrastructure (Weeks 1-2)

### 1.1 Repository & Monorepo Setup
- [ ] Initialize Git monorepo (Turborepo or Nx recommended).
    - `apps/web` (React/Vite)
    - `apps/mobile` (React Native/Expo)
    - `apps/extension` (Chrome/Edge)
    - `packages/ui` (Shared Design System)
    - `services/api` (Go/Chi Gateway)
    - `services/agents` (Go/Genkit Agent Runners)
    - `packages/shared-types` (TypeScript/gRPC protos)
- [ ] Configure `eslint`, `prettier`, `typescript` (Strict Mode).
- [ ] Set up pre-commit hooks (Husky).

### 1.2 Infrastructure (Kubernetes Setup)
- [ ] Provision **Kubernetes Cluster** (GKE, EKS, or Managed K8s).
- [ ] Set up **Terraform/Pulumi** for IaC (Infrastructure as Code).
- [ ] Configure **Helm** for service deployments.
- [ ] Provision **NATS JetStream** (3-node cluster) for durable event bus.
- [ ] Set up **Redis** (for Supervisor session state and ratelimiting).
- [ ] Define **AgentContext Protobuf/JSON Schema** for standardized inter-agent communication.
- [ ] Set up **PostgreSQL (HA)** with pgvector.
- [ ] Set up **Object Storage** (Cloudflare R2).
- [ ] Set up **Ingress Controller** (Nginx/Traefik) with SSL.
- [ ] Set up **Horizontal Pod Autoscaling (HPA)** base configuration.
- [ ] Set up **CI/CD Pipelines** (GitHub Actions):
    - Lint/Test on PR.
    - Build & Push Docker images to Registry (GHCR/GCR).
    - Deploy to K8s via Helm on merge to `main`.

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
- [ ] Implement **Offline Storage Bridge**: SQLite/RxDB foundation for local sync.

---

## Phase 2: AI Agent Infrastructure & Search (Weeks 3-4)

### 2.1 Google Genkit & Event Implementation (Go)
- [ ] Initialize Go Workspace for backend services.
- [ ] Install and configure **Google Genkit (Go SDK)**.
- [ ] Set up **Vertex AI** integration (Gemini models via Go SDK).
- [ ] Implement **Go Event Consumers** (Goroutine pools for Redis Streams/NATS).
- [ ] Implement **Model Router Service** (Stateless Go service with YAML-based provider config).
- [ ] Implement **Safety Service** (Go) as a mandatory gateway for all external agent outputs.
- [ ] Implement `DeepResearch` agent (Async Job pattern with partial updates).
- [ ] Implement **AI Decision Log** (Async write to ClickHouse/BigQuery).
- [ ] Implement **Internet Search Tool** (Tavily, Google, or Brave Search integration).
- [ ] Configure **A2A protocol** via Event Bus.

### 2.2 Supervisor Agent
- [ ] Implement **Supervisor Agent** (Central Chief of Staff).
- [ ] Intent classification: route to Synthesis, Search, or Learning agents.
- [ ] Session state management for multi-hop research tasks.

### 2.3 Safety Agent (Critical Path)
- [ ] Implement **Safety Agent** (Mandatory output filter).
- [ ] Medical safety policy engine (Educational vs Clinical boundaries).
- [ ] Citation verification for web-sourced results.

### 2.4 Research Tool Layer
- [ ] **Vertex AI Tool**: LLM model access with multi-provider fallback.
- [ ] **Deep Research Tool**: Multi-hop web search and synthesis engine.
- [ ] **Search Tool**: Rapid internet search for factual queries.
- [ ] **Database Tool**: CRUD for user research history.
- [ ] **Calendar Tool**: ICS export for study tasks.

### 2.5 API Gateway (Go)
- [ ] Set up **Go API Gateway** (using Chi or Gin router).
- [ ] Implement **Goroutine-based** request handling for high concurrency.
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
- [ ] **HPA Custom Metrics** (Scaling pods based on Event queue depth).


---

## Phase 3: Core Research Agents (Weeks 5-8)

### 3.1 Core Tutor & Pedagogy Agents
- [ ] Implement **Tutor Agent** (Socratic, Layered explanations).
- [ ] Implement **Socratic Agent** ("The Attending" - Diagnostic Debrief logic).
- [ ] Implement **Retrieval Agent** (Web + Library grounding).
- [ ] Implement **Voice Agent** (Deepgram/ElevenLabs integration for OSCE).
- [ ] Implement **Vision Agent** (Radiology/Dermatology Analysis).

### 3.2 Assessment & Active Recall Agents
- [ ] Implement **Quiz Agent** (Distractor rationales, Bloom's taxonomy).
- [ ] Implement **Flashcard Agent** (Anki-style card generation).
- [ ] Implement **Simulator Agent** (Patient state machine, vitals tracking).

### 3.3 Study Success Agents
- [ ] Implement **Planner Agent** (Spaced Repetition Schedule, Calendar Sync).
- [ ] Implement **UserProfile Agent** (Knowledge Tracing, Adaptive Learning Model).
- [ ] Implement **Research Agent** (PubMed/Journal Club Helper).
- [ ] Implement **Format Agent** (Summarizer, PDF/Markdown Converter).


---

## Phase 4: Chat-First Web Client (Weeks 9-11)

**Principle:** Mobile-first implementation. Build for small screens first, then enhance for desktop.

### 4.1 App Shell & Navigation
- [ ] mobile-first CSS architecture.
- [ ] Sidebar/Navbar (Responsive with Bottom Nav for mobile).
- [ ] Layouts (Dashboard, Focus Mode).
- [ ] Dark/Light Mode Toggles (Tailwind).

### 4.2 Core Workflows
- [ ] **Onboarding Flow**:
    - Goals, Exam details, Language selection.
- [ ] **Integrated Chat Interface**:
    - Streaming responses from Research Agents.
    - Research Drawer for citations and multi-hop synthesis steps.
- [ ] **Dashboard**:
    - Research history, streaks, and progress.

### 4.3 Study Interfaces
- [ ] **Flashcard Runner**:
    - Flip animation (CSS 3D).
    - SRS Feedback buttons (Again/Hard/Good/Easy).
- [ ] **Quiz Runner**:
    - Timer/Untimed modes.
    - Explanation reveal state.
    - "Flag" question feature.

### 4.4 Offline-First Architecture ("Hospital Mode")
- [ ] Implement **PowerSync** (Go Service + Client SDK) for robust offline data.
- [ ] Define **SQLite Local Schema** for Flashcards, Notes, and Profile.
- [ ] Build **Resilient Retry Queue** for offline actions.
- [ ] Sync Strategy: Last-Write-Wins for simple data, delta-updates for Study History.

### 4.5 Multiplayer ("Virtual Rounds")
- [ ] Implement **NATS WebSockets** gateway for real-time signaling.
- [ ] Build **Session State Manager** (Dragonfly) for shared simulation state.
- [ ] Frontend: Live user presence and shared patient vitals view.

---

## Phase 5: Document Ingestion (Additional) (Weeks 12-13)

### 5.1 Upload & Ingestion Pipeline
- [ ] Implement `POST /v1/uploads`.
- [ ] Create **Background Worker** for document processing (OCR, Chunking, etc.).

### 5.2 Vector Indexing
- [ ] Implement Embedding generation and pgvector storage.
- [ ] Link user context to the Retrieval Agent for personalized research.

---

## Phase 6: Agent Studio & A2A (Weeks 14-16)

### 6.1 Agent Builder UI
- [ ] **YAML Mode**: Code-first agent definition interface
- [ ] Instruction builder with templates
- [ ] Tool selection interface (Research Tools, Search Tools)
- [ ] Data source configuration (External URLs, Library Docs)

### 6.2 Agent Templates
- [ ] Built-in templates (Specialty Tutor, Research Specialist, Case Analyst)
- [ ] Template cloning and customization

### 6.3 Agent Sharing & Marketplace
- [ ] Agent visibility settings (private, cohort, public)
- [ ] Community agent browsing and search
- [ ] Agent installation (one-click add)

---

## Phase 7: Mobile & Extension (Weeks 17-18)

### 7.1 Mobile App (React Native/Expo) - Full Feature Parity
- [ ] Setup Navigation (Expo Router).
- [ ] Screen Parity (All Web Features):
    - Home (Dash).
    - Reviews (SRS Runner - Swipe gestures).
    - Chat (Integrated Research Agents).
    - Library Management.
    - Settings & Profile.
- [ ] Push Notifications (Expo Notifications).

### 7.2 Browser Extension
- [ ] Manifest v3 Setup.
- [ ] Content Script:
    - Highlight medical terms.
    - "Ask MedMentor Agent" context menu.
- [ ] Popup:
    - Mini Chat integrated with Research Agents.

---

## Phase 8: Optimization & Launch (Weeks 19-20)

### 8.1 Institutional Interop (B2B)
- [ ] Implement **LTI 1.3 Provider** adapter (Go).
- [ ] Implement **xAPI (Tin Can)** logger for learning events.
- [ ] Data Export compliance (GDPR/FERPA).

### 8.2 Security & Compliance
- [ ] Rate Limiting audit.
- [ ] Content Safety (Medical advice) Red-teaming.

### 8.2 Analytics & Monitoring
- [ ] Set up Telemetry (PostHog).
- [ ] AI Evals (Citation accuracy, Search relevance).

### 8.3 Final QA & Deployment
- [ ] E2E Testing (Playwright).
- [ ] **Production Deploy**:
    - VPS final optimization and backup verification.

---

## Dependencies & Risks

### Dependencies
- **LLM API Availability / Cost**: high volume of tokens for research synthesis.
- **Search API Costs**: Iterative search tasks can scale costs quickly.
- **Design System Fidelity**: Glassmorphism must be optimized for performance.

### Key Risks
- **Hallucinations**: AI inventing medical facts. Implementation of strict Grounding is P0.
- **Latency**: Deep research tasks can be slow. Needs aggressive progress tracking and streaming.
- **Search Accuracy**: Dependence on web-sourced medical information requires high authority ranking.
