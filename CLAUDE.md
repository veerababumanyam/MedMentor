# MedMentor AI - Claude Conversation Summary

**Project:** MedMentor AI - Medical Education Platform
**Architecture:** Google ADK Agent-First Backend
**Last Updated:** 2026-02-19
**Version:** 0.0.1

---

## Project Overview

MedMentor AI is an AI-powered medical education platform designed as an **agent-first architecture** using Google Agent Development Kit (ADK) with A2A (Agent-to-Agent) protocol. The system provides:

- **AI Tutor Agent** with layered explanations (Simple → Student → Clinical)
- **RAG-based content retrieval** with verified citations
- **Quiz and Flashcard generation** with spaced repetition
- **Study scheduling** with adaptive planning
- **Agent Studio** - users can create custom AI agents (v0.1 feature)

---

## Architecture Philosophy: Agent-First

### Core Principle

All backend capabilities are delivered through AI agents orchestrated by a Supervisor Agent, not through traditional API endpoints.

```
Client → API Gateway (Lightweight) → Supervisor Agent
    → Specialist Agents → Safety Agent → Response
```

### Google ADK Framework

- **Framework:** Google Agent Development Kit (ADK) v1.25+
- **Protocol:** A2A (Agent-to-Agent) for inter-agent communication
- **Languages:** Python (primary), TypeScript/Node.js (alternative)
- **LLM Provider:** Vertex AI (Gemini) with LiteLLM multi-provider fallback
- **Documentation:** [ADK Docs](https://google.github.io/adk-docs/)

---

## Agent Hierarchy

```
Supervisor Agent (Root)
├── Safety Agent (All paths converge here)
├── Tutor Agent
├── Retrieval Agent
├── Quiz Agent
├── Flashcard Agent
├── Scheduler Agent
├── Analytics Agent (A2A - remote)
└── Moderation Agent
```

### Agent Responsibilities

| Agent | Responsibility |
|-------|---------------|
| **Supervisor** | Route requests, coordinate agents, aggregate responses |
| **Safety** | Validate ALL outputs, enforce refusals, verify citations |
| **Tutor** | Explanations, Socratic method, layered depth |
| **Retrieval** | RAG search, citations, source verification |
| **Quiz** | Question generation, distractor rationales |
| **Flashcard** | Card creation, deduplication, SRS data |
| **Scheduler** | Study planning, adaptive rescheduling |
| **Analytics** | Progress tracking, insights (A2A remote) |

---

## Technology Stack

### Backend
- **Framework:** Python 3.11+ or TypeScript/Node.js 20+
- **Agent Orchestration:** Google ADK
- **API:** FastAPI (Python) or Express (Node.js)
- **LLM:** Vertex AI (Gemini 2.5 Flash/Pro)
- **Model Router:** LiteLLM (multi-provider fallback)

### Data & Storage
- **Database:** PostgreSQL 16+ with pgvector
- **Vector Store:** Vertex AI Vector Search or pgvector
- **Cache:** Redis
- **Object Storage:** Cloudflare R2 or Cloud Storage

### Frontend
- **Framework:** React with Vite
- **Styling:** Tailwind CSS + CSS Modules (glassmorphism)
- **State:** React Context or Zustand/Jotai
- **Routing:** React Router

---

## A2A Protocol (Agent-to-Agent)

The A2A protocol enables:
- **Scalability:** Run agents on separate servers
- **Modularity:** Independent agent development and deployment
- **Flexibility:** Mix local and remote agents seamlessly
- **Security:** Authenticated agent-to-agent communication

### A2A Remote Agents

```python
from google.adk.agents import RemoteAgent

# Remote analytics agent
analytics_agent = RemoteAgent(
    name="AnalyticsAgent",
    endpoint="https://analytics.medmentor.com/a2a",
    auth_token=os.getenv("A2A_AUTH_TOKEN")
)
```

---

## Agent Studio (v0.1 Feature)

Users can create custom AI agents through:
1. **YAML Mode** - Code-first agent definition
2. **Visual Builder** - Drag-and-drop interface (optional for v0.1)
3. **Template Library** - Pre-built agent templates
4. **Testing & Validation** - Safety checks before deployment
5. **Marketplace** - Share agents with the community

### Example Custom Agent

```yaml
name: "Cardiology Tutor"
description: "Specialized tutor for cardiovascular topics"

instruction: |
  You are a Cardiology Tutor specializing in cardiovascular medicine.
  Focus on: anatomy, conditions, diagnostics, treatments.
  Always use clinical vignettes in explanations.

tools:
  - vector_search
  - citation_tool
  - guideline_database

data_sources:
  - user_uploads
  - cardiology_guidelines

behavior:
  depth_level: "student"
  bilingual: false
  citation_required: true
```

---

## Safety Rules (NON-NEGOTIABLE)

All AI agent outputs must:

1. **REFUSE** patient-specific diagnosis or treatment recommendations
2. **REFUSE** emergency medical guidance (redirect to emergency services)
3. **REFUSE** specific dosage recommendations for individuals
4. **VERIFY** all citations are real and support the claims
5. **ADMIT** uncertainty when evidence is weak or conflicting
6. **PROVIDE** safe alternatives: general explanations, guideline summaries

**All paths must route through Safety Agent before responding to users.**

---

## Documentation Files

### Core Documentation
- **[docs/PRD.md](docs/PRD.md)** - Product Requirements with agent-first philosophy
- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** - Complete ADK/A2A architecture
- **[docs/PROJECT_PLAN.md](docs/PROJECT_PLAN.md)** - 18-20 week implementation plan
- **[docs/AI_RULES.md](docs/AI_RULES.md)** - Development rules and guidelines

### Agent Documentation
- **[docs/AGENTS.md](docs/AGENTS.md)** - Complete agent catalog with ADK patterns
- **[docs/A2A_PROTOCOL.md](docs/A2A_PROTOCOL.md)** - A2A protocol specifications
- **[docs/AGENT_STUDIO.md](docs/AGENT_STUDIO.md)** - User agent builder specifications
- **[docs/ADK_SETUP.md](docs/ADK_SETUP.md)** - ADK setup and configuration guide

### Design & Governance
- **[docs/DESIGN.md](docs/DESIGN.md)** - UI/UX design system (glassmorphism, mobile-first)
- **[Constitution](.specify/constitution.md)** - Project governance and principles

---

## Development Workflow

1. **Context Discovery:** Review PRD, ARCHITECTURE, and DESIGN docs
2. **Skill Application:** Leverage Antigravity Skills (`.agent/skills`) and Stitch Skills (`.agents/skills`)
3. **Plan:** Create implementation plan for complex features
4. **Implement:** Write clean, tested code following AI_RULES.md
5. **Verify:** Check design compliance, responsiveness, accessibility, error handling
6. **Update Docs:** Keep documentation in sync with code

---

## Key Design Principles

### Glassmorphism UI
- Standard classes: `.glass`, `.glass-elevated`
- Trust Blue: `#0056D2`
- Growth Teal: `#00E8C6`
- Midnight Navy: `#0F172A`

### Mobile-First
- 4px grid alignment
- Minimum 44x44px touch targets
- 2-5 minute session optimization

### Internationalization
- Support for English, Spanish, Arabic (RTL)
- Externalized strings
- Locale-aware formatting

---

## Project Status

**Current Phase:** Foundation & Planning (Phase 1)

**Completed:**
- ✅ Documentation transformation to Google ADK agent-first architecture
- ✅ Complete agent catalog with A2A patterns
- ✅ Agent Studio specifications (v0.1 feature)
- ✅ ADK setup guide

**Next Steps (Phase 2 - ADK Agent Infrastructure):**
- Set up Google ADK (Python or TypeScript)
- Implement Supervisor Agent
- Implement Safety Agent (critical path)
- Set up core tools (Vertex AI, Vector, Database)
- Create lightweight API Gateway

---

## Implementation Timeline (18-20 Weeks)

| Phase | Focus | Duration | Key Deliverables |
|:---|:---|:---|:---|
| **1** | Foundation | W1-2 | Repo, Infrastructure, DB, Auth, Design Tokens |
| **2** | ADK Agent Infrastructure | W3-4 | Google ADK setup, Supervisor, Tool Layer, A2A |
| **3** | Core Learning Agents | W5-8 | Tutor, Retrieval, Safety, Quiz, Flashcard Agents |
| **4** | Agent Studio | W9-10 | Agent Builder UI, Templates, Testing, Sharing |
| **5** | Tool Integration & A2A | W11-12 | MCP, A2A remote agents, Calendar, Search |
| **6** | Web Client | W13-15 | Dashboard, Study Interfaces, Library |
| **7** | Mobile & Extension | W16-18 | Mobile App, Browser Extension |
| **8** | Polish & Launch | W19-20 | Security Audit, Load Testing, Production Deploy |

---

## Quick Reference

### Run a Tutor Agent

```python
from google.adk.agents import LlmAgent

tutor_agent = LlmAgent(
    name="TutorAgent",
    instruction="""
    You are a medical education Tutor using the Socratic method.
    Provide explanations at three depth levels: Simple, Student, Clinical.
    Always cite sources and admit uncertainty.
    """,
    tools=[
        Tool("search_knowledge", search_knowledge_function),
        Tool("get_citations", get_citations_function)
    ],
    model="gemini-2.5-flash",
    output_key="explanation"
)

response = await tutor_agent.invoke({
    "input": "Explain myocardial infarction",
    "user_id": "user_123",
    "depth_level": "student"
})
```

### A2A Remote Agent Call

```python
# Supervisor calling remote Analytics Agent via A2A
result = await analytics_agent.invoke({
    "action": "get_progress_insights",
    "user_id": user_id
})
```

---

## Resources

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [A2A Protocol Quickstart](https://google.github.io/adk-docs/a2a/quickstart-consuming)
- [Vertex AI Documentation](https://cloud.google.com/vertex-ai)
- [Google Gen AI Python SDK](https://github.com/googleapis/python-genai)

---

## License & Proprietary Info

[See docs/PRD.md Section 11 for full licensing details]
