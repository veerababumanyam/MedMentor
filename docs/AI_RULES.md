# MedMentor AI — AI & Development Rules

**Version:** 0.0.1 (Google ADK)
**Last Updated:** 2026-02-19

This document outlines the mandatory rules and guidelines for AI agents and developers working on MedMentor AI. These rules are derived from `docs/PRD.md`, `docs/ARCHITECTURE.md`, and `docs/DESIGN.md`. All contributions must adhere to these standards to ensure quality, consistency, and safety.

---

## 1. Core Philosophy & Vision

*   **Agent-First Architecture:** All backend capabilities delivered through Google ADK agents with A2A protocol
*   **Trust is a Feature:** Every potential medical fact must be grounded in retrieval (RAG) with citations. Never fabricate citations. Uncertainty must be explicit.
*   **Safety First:** The system must strictly refuse patient-specific diagnosis or treatment advice. It is an *educational* tool, not a clinical decision support system.
*   **Privacy by Design:** Minimize data collection. Isolate user workspaces. Treat uploaded content as sensitive.
*   **Mobile-First Learning:** Workflows must optimize for short (2-5 min) mobile sessions.
*   **Internationalization (i18n):** The codebase must support i18n from day one (externalized strings, RTL ready, locale-aware formatting).
*   **Custom Agents:** Users can create their own agents via Agent Studio (v0.1 feature).

---

## 2. Technology Stack & Architecture

### 2.1 Agent Orchestration (Google ADK)
*   **Framework:** Google Agent Development Kit (ADK) v1.25+
*   **Protocol:** A2A (Agent-to-Agent) for inter-agent communication
*   **Languages:** Python (primary), TypeScript/Node.js (alternative), Java, Go
*   **LLM Provider:** Vertex AI (Gemini) with LiteLLM multi-provider fallback
*   **Agents:** Supervisor, Tutor, Retrieval, Quiz, Flashcard, Safety, Scheduler, Analytics
*   **Documentation:** [ADK Docs](https://google.github.io/adk-docs/)

### 2.2 Web Application
*   **Framework:** React (Vite).
*   **Language:** TypeScript (Strict mode).
*   **Styling:**
    *   **Tailwind CSS** for layout and utilities.
    *   **CSS Modules / Vanilla CSS** for complex glassmorphism effects and animations.
*   **State Management:** React Context or lightweight library (Zustand/Jotai) preferred over Redux unless complexity demands it.
*   **Routing:** React Router.

### 2.3 Backend & API
*   **Architecture:** Agent-First with lightweight API Gateway (FastAPI or Express)
*   **API Style:** RESTful resources (`/v1/...`) with streaming support
*   **Auth:** OAuth (Google/Apple/Microsoft) with PKCE.
*   **Database:** PostgreSQL 16+ with pgvector (primary + vector in one)
*   **Vector Store:** Vertex AI Vector Search or pgvector
*   **Cache:** Redis for session cache and RAG results
*   **Object Storage:** Cloudflare R2 or Cloud Storage

### 2.4 Tool Integration
*   **Native:** ADK Tools for all external access
*   **Optional:** MCP (Model Context Protocol) 1.3+ for extensibility
*   **A2A:** Agent-to-Agent protocol for remote agents

---

## 3. UI/UX Design Rules

Follow `docs/DESIGN.md` explicitly.

### 3.1 Visual Language
*   **Glassmorphism:** Use standard classes (`.glass`, `.glass-elevated`) for depth.
*   **Typography:** 
    *   **Primary:** Inter.
    *   **Display:** Plus Jakarta Sans.
    *   **Monospace:** JetBrains Mono / SF Mono.
*   **Color Palette:** 
    *   **Trust Blue:** `#0056D2`
    *   **Growth Teal:** `#00E8C6`
    *   **Midnight Navy:** `#0F172A`
*   **Themes:** Full support for Light and Dark (Glass) modes.

### 3.2 Layout & Spacing
*   **Grid:** Everything aligns to a **4px grid**.
*   **Touch Targets:** Minimum **44x44px** for all interactive elements.
*   **Responsiveness:** 
    *   Mobile: 100% width, 16px padding.
    *   Tablet: Max 768px.
    *   Desktop: Max 1200px.

### 3.3 Components
*   Use shared, reusable components. Do not build ad-hoc styles.
*   Interactive elements must have visible focus rings for accessibility (WCAG 2.2 AA).
*   Animations must respect `prefers-reduced-motion`.

---

## 4. Coding Standards

### 4.1 General
*   **Type Safety:** No `any`. Use strict typing for all interfaces, props, and return types.
*   **Comments:** Document complex logic. Explain *why*, not just *what*.
*   **Error Handling:** Fail gracefully. User-facing errors must be localized and helpful.

### 4.2 Frontend
*   **Functional Components:** Use React Hooks. Avoid class components.
*   **Performance:**
    *   Optimize images and assets.
    *   Lazy load routes and heavy components.
    *   Be mindful of `backdrop-filter` performance implications (provide fallbacks).
*   **Accessibility:**
    *   Semantic HTML (`<main>`, `<nav>`, `<article>`).
    *   `aria-labels` for icon-only buttons.
    *   Ensure color contrast ratios met (4.5:1 text).

### 4.3 Backend
*   **Validation:** Validate all inputs (Zod/Joi).
*   **Security:** Sanitize inputs to prevent injection. Implement rate limiting.
*   **Logging:** Structured logging for observability.

---

## 5. Development Workflow

1.  **Check Documentation:** Before starting a task, verify requirements against `docs/PRD.md` and visuals against `docs/DESIGN.md`.
2.  **Plan:** Create a brief implementation plan for complex features.
3.  **Implement:** Write clean, tested code.
4.  **Verify:**
    *   Does it meet the design spec?
    *   Is it responsive?
    *   Is it accessible?
    *   Does it handle errors/loading states?
5.  **Update Docs:** If you change architecture or design patterns, update the corresponding documentation.

---

## 6. ADK Agent Development Rules

### 6.1 Agent Definition Pattern

Every ADK agent must have:

```python
from google.adk.agents import LlmAgent

agent = LlmAgent(
    name="AgentName",              # Unique, descriptive name
    instruction="...",             # Clear role and behavior
    tools=[...],                    # Available tools
    output_key="result_key",        # State key for output
    model="gemini-2.5-flash"        # Via model router
)
```

### 6.2 Agent Responsibilities

| Agent | Responsibility |
|-------|---------------|
| **Supervisor** | Route requests, coordinate agents, aggregate responses |
| **Safety** | Validate ALL outputs, enforce refusals, verify citations |
| **Tutor** | Explanations, Socratic method, layered depth |
| **Retrieval** | RAG search, citations, source verification |
| **Quiz** | Question generation, distractor rationales |
| **Flashcard** | Card creation, deduplication, SRS data |
| **Scheduler** | Study planning, adaptive rescheduling |

### 6.3 A2A Communication Rules

*   **Use A2A for:** Scaling, modularity, independent deployment
*   **Local first:** Keep agents local unless scaling requires remote deployment
*   **Authentication:** All A2A calls use bearer tokens
*   **Timeout handling:** 30s default timeout with exponential backoff
*   **Circuit breaker:** Open circuit after N consecutive failures

### 6.4 Safety Rules (NON-NEGOTIABLE)

*   **All paths must route through Safety Agent** before responding to users
*   **Never fabricate citations** - if retrieval fails, admit uncertainty
*   **Refuse patient-specific advice** - redirect to educational content
*   **Bilingual mode for high-risk non-English content**
*   **Log all agent executions** for observability and audit trails

### 6.5 Tool Development Rules

*   All external access via ADK tools (no direct API calls from agents)
*   Tools must have explicit schemas and descriptions
*   Implement timeout and retry logic for all tools
*   Validate tool outputs before using in agent responses

### 6.6 Testing Requirements

*   **Unit tests:** Each agent tested in isolation
*   **Integration tests:** Agent handoffs and A2A communication
*   **Safety tests:** Refusal correctness, citation accuracy
*   **Evals:** See `docs/PRD.md` §6.4 for evaluation thresholds

---

## 7. Agent Studio Rules (User-Created Agents)

### 7.1 User Agent Validation

*   **Safety check:** All user agents pass automated safety validation
*   **Template restrictions:** Limited to safe instruction patterns
*   **Tool restrictions:** Users cannot access unsafe tools
*   **Data isolation:** User agents only access their own data

### 7.2 Community Agents

*   **Verification:** Featured agents require clinician/educator verification
*   **Moderation:** Community-reported agents reviewed
*   **Rating system:** Users rate agents for quality and safety

---

## 8. Development Workflow (Updated)
