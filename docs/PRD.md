# MedMentor AI — Product Requirements Document (PRD)

**Document status:** 
**Version:** 0.0.1

| Field | Value |
|---|---|
| Product | MedMentor AI |
| PRD owner | Veera Babu Manyam |
| Last updated | 2026-02-19 |
| Target launch | 2026-03-15 |
| Primary platforms | Web, iOS, Android |

---

## 1. Summary

### 1.1 Vision
Empower medical learners through a **LLM-based, Search-First, AI-driven study system** that uses deep research and internet-connected specialists to turn any query or clinical scenario into an adaptive learning path. While users can ingest their own documents, the core experience is driven by agents that synthesize global medical knowledge into personalized insights.

### 1.2 Problem statement
Medical learners face:

1. **Information overload** (dense lectures, notes, textbooks).
2. **Low retention** from passive study; effective learning requires *active recall* and *spaced repetition*.
3. **Exam mismatch** across regions (USMLE, PLAB, NEET PG, UKMLA, etc.).
4. **Clinical reasoning gap** from memorization to differential diagnosis.
5. **Trust constraints**: incorrect medical info (e.g., dosing) can cause real harm.

### 1.3 Goals
* Provide a **research-backed learning loop**: search/query → synthesis → quiz → flashcards → schedule.
* Deliver **evidence-linked** outputs grounded in real-time internet search and deep research, with transparent uncertainty.
* Implement a formal **Evidence Hierarchy**: Weight peer-reviewed journals (PubMed, JAMIA) and established guidelines (UpToDate, NICE) higher than general web results.
* Support **multimodal learning** (PDFs, images, audio) as an enhancement to the core AI-driven research.
* Achieve **safe-by-design** behavior for medical domains (no patient-specific medical advice).
* Support **multilingual learning**: AI outputs are translated and grounded in the learner's preferred language.
* **Agent-First architecture**: A chat-centric frontend integrated with specialized backend AI agents that perform deep research, internet search, and task orchestration.
* **Offline Resilience (v0.1)**: "Local Cache + Resilient Retry" strategy. Ensure flashcards and recently viewed content are available offline; defer full offline RAG to v0.2.

### 1.3.1 Agent-First Product Philosophy (NEW)

**Core Philosophy**: MedMentor is built as an AI agent-driven system from the ground up. Every user interaction is handled by specialized AI agents that collaborate through **Google Genkit (Go)** and the Agent-to-Agent (A2A) protocol.

**Key Architectural Principles**:

1. **Supervisor Orchestrator**: A central Supervisor Agent routes all user requests to appropriate specialist agents, managing handoffs and ensuring consistent responses.

2. **Specialized Agents**: Each core capability is delivered by a dedicated agent:
   - **Tutor Agent**: Layered explanations using Socratic method
   - **Retrieval Agent**: Grounded content with citations via RAG
   - **Quiz Agent**: Question generation with distractor rationales
   - **Flashcard Agent**: Card creation with SRS scheduling data
   - **Safety Agent**: Policy enforcement and refusal logic (all outputs route through)
   - **Scheduler Agent**: Adaptive study planning and rescheduling
   - **Analytics Agent**: Progress tracking and recommendations

3. **A2A Protocol**: Agent-to-Agent communication enables:
   - Scalability: Run agents on separate servers
   - Modularity: Independent agent development and deployment
   - Flexibility: Mix local and remote agents seamlessly
   - Security: Authenticated agent-to-agent communication

4. **Agent Studio (v0.1)**: Immersize "Notebook" workspaces. Users upload sources (PDF, text, images) to create dedicated **A2A Specialist Agents**.
   - Grounded RAG with visual pinpointing (PDF highlights).
   - "Handy Tools" bar for rapid summarization and clinical correlation.
   - Admin-led model routing (Gemini 3 Pro / GPT-5).
   - Setting behavior parameters
   - Sharing agents with the community
   - Validating agents before deployment

5. **Tool Integration**: Native ADK tools with optional MCP (Model Context Protocol) support for extensibility.

### 1.3.2 Product USP (updated)
**The Research-First Medical Brain**: A system where the primary interface is a chat integrated with agents that have real-time access to global medical knowledge via internet search and deep research capabilities. User-provided content (EHRs, notes, PDFs) acts as a contextual layer that enhances the AI's primary research.

Core loop:
1. **Query & Research**: User input (question, topic, or scenario) triggers deep research and internet search via specialized agents.
2. **Personalize (Optional Ingestion)**: User can upload sources (PDF, text) or clinical notes to ground the AI's research in their specific curriculum.
3. **Generate Assets**: AI synthesizes research into summaries, mind maps, quizzes, and flashcards.
4. **Practice & Remediate**: Practice questions with distractor rationales; focus on gaps via missed-concept remediation.
5. **Schedule & Reinforce**: Spaced repetition to ensure long-term mastery.

### 1.4 Non-goals (initially)
* Diagnosing real patients or offering patient-specific treatment recommendations.
* Acting as a regulated clinical decision support tool for patient care workflows.
* Hosting or redistributing copyrighted textbooks / commercial Q-bank content.

---

## 2. Users, personas, and jobs-to-be-done

### 2.1 Primary personas

1. **Pre-clinical student (Year 1–2)**
   * Needs: concept clarity, retention, exam-style questions, habit building.
2. **Clinical student (Year 3–4)**
   * Needs: clinical reasoning, OSCE practice, concise workflow tools.
3. **Intern/Resident / postgrad exam prep**
   * Needs: high-yield review, rapid refreshers, targeted remediation.

### 2.2 Secondary personas

1. **Educator / institution admin (B2B)**
   * Needs: cohort analytics (aggregated), curriculum gap detection, assignment support.
2. **Content author / reviewer**
   * Needs: tooling to curate decks/cases, audit outputs, manage quality.

### 2.3 Core jobs-to-be-done (JTBD)
* “When I’m stuck on a topic, I want a layered explanation so I can understand and remember it.”
* “When I study, I want a plan that adapts when I fall behind, without burning out.”
* “When I practice questions, I want to learn from mistakes and focus on my gaps.”
* “When I practice clinically, I want safe simulation with feedback, not guesswork.”

---

## 3. Competitive landscape (feature-derived)

MedMentor should meet or exceed “table stakes” found in modern med-ed tools:

* **Study schedule + adaptive rescheduling** (common in commercial platforms).
* **Quiz / question bank with explanations and progress tracking**.
* **Spaced repetition flashcards** and active recall loops.
* **Mobile-first workflows** for studying in short bursts.

External references (non-exhaustive):
* Osmosis feature set emphasizes study schedules, quiz builders, flashcards, and progress feedback.
* Anki emphasizes sync, media-rich cards, customization, and large deck performance.

Competitor-derived “killer features” (to include in MedMentor):
* **Exhaustive distractor rationales**: not only why the correct answer is correct, but why each incorrect option is wrong, with high-yield visuals where permitted.
* **Term hover / instant definitions**: hovering a medical term shows a mini high-yield card and links to deeper library content (no page-switching).
* **Browser extension**: highlight medical terms on *any* website and open MedMentor “term cards” + citations + saved notes.
* **Community collaboration**: shared decks and shared tagging standards, with versioning and moderation (institution or cohort based).
* **Visual mnemonic generation**: safe, non-infringing, memorable visuals to improve recall for high-yield topics (e.g., pharm, micro).

Multilingual “table stakes”:
* **Multilingual UX**: students can study in English by default or switch to their preferred language; core content should remain medically precise.
* **RTL support** for Arabic (and future RTL languages).
* **Bilingual safety mode**: show English alongside translations to reduce mistranslation risk.

---

## 4. Product scope



### 4.1 Must Have v0.1

**Learning core**
1. **AI Agent Chat**: Primary interface integrated with backend research agents
2. **Deep Research & Internet Search**: Core capability for answering complex medical queries
3. **Evidence-linked answers**: Citations to real-time web sources and user documents
4. **Flashcards + spaced repetition queue**: (native)
5. **Quiz builder**: (topic-based) + explanations
6. **Study schedule**: (calendar-like checklist + auto-reschedule)
7. **Document Ingestion (Additional Feature)**: Upload + “chat with my notes” (PDF, DOCX, text)
8. **Evidence Tier Dashboard**: Visual indicator of search result authority (Level 1: RCT/Meta-analysis → Level 5: Case report/Web).
9. **Curriculum Mapping**: Grounding research in a specific uploaded syllabus.

**Trust & safety**
1. “Not medical advice” guardrails + patient-safety refusals
2. Hallucination controls: “cannot verify” behavior
3. User reporting (bad answer / unsafe content / IP violation)

**Account & platform**
1. Authentication (email + OAuth)
  * OAuth providers (v0.1 target): **Google**, **Apple**, **Microsoft** (OIDC)
  * OAuth must use PKCE where applicable and follow platform best practices for secure token storage
2. Cross-device sync
3. Account switching support (shared devices)
  * Users can switch accounts without data leakage between accounts
  * Clear “current account” indicator in settings
4. Basic analytics dashboard (streaks, accuracy, review load)
5. Search + organization: global search, tags, folders, and filters
6. Notifications: review reminders + study schedule nudges (email/push, user-configurable)
7. Settings: notification preferences, privacy controls, **language/locale selection**, and data export/delete
8. In-app support: FAQs + contact/support form

**Globalization / i18n (foundation)**
1. English default with **i18n-ready UI** (string externalization, plural rules, locale-aware dates/times)
2. Ability for users to set preferred **language** and **region** (e.g., en-US vs en-GB)

**Commercial (if monetized at MVP)**
1. Subscriptions (web + mobile), receipts/invoices
2. Entitlements + quotas (uploads, generations, storage)
3. Graceful paywalling (no data loss)

**Agent Studio (NEW)**
1. Custom agent creation interface (YAML or visual builder)
2. Agent template library for common study workflows
3. Agent testing and validation before deployment
4. Agent sharing and marketplace (community agents)
5. Agent versioning and rollback support

#### 4.1.1 Priority language rollout v0.1
Rationale: maximize coverage of large learner populations and high smartphone adoption, while keeping localization scope manageable.

Decision note (2026-02-19): user preference is to support **all languages**. Implementation approach:
* **UI localization** ships in tiers (below) for quality and ops scalability.
* **AI outputs** support “any language” on a best-effort basis, with stronger safety defaults (bilingual output, extra uncertainty labeling) for non-tier languages.

Tier 0 (required):
* **English** (default; global medical lingua franca)

Tier 1 (highest ROI early):
* **Spanish** (large global student base across Latin America + Spain)
* **Portuguese (Brazil)**
* **French** (Europe + Africa)
* **Arabic** (MENA; requires RTL)

Tier 2 (next wave):
* **Hindi** (India)
* **Indonesian** (Indonesia)
* **Russian**
* **Turkish**

Tier 3 (later / validate demand):
* **Mandarin Chinese (Simplified)**
* **Bengali**
* **Urdu**
* **German** (often English content is acceptable, but UI localization may help)
* **Japanese**
* **Korean**

Implementation note: language tiering should be validated with waitlists, school partnerships, and geo analytics.

### 4.2 V0.2
1. Multimodal: image-based learning (diagrams, radiology practice sets) with safe disclaimers
2. OSCE / virtual patient text simulation
3. Anki export + import (APKG/CSV where feasible)
4. Institutional (B2B) beta: cohort dashboards with strong privacy controls
5. Knowledge Library + **hover term cards** (cross-linked glossary)
6. **Browser extension** (Chrome/Edge) for term highlighting + quick lookups + “save to MedMentor”
7. **Community Hub** for collaborative decks (cohort/university workspaces)
8. AI **visual mnemonic generator** (image-based memory aids)
9. **Depth-on-demand** from uploads: hierarchical outlines + interactive **mind maps** + instant quizzes
10. **Multilingual mode**: generate summaries/quizzes/flashcards in the user’s preferred language (with bilingual options)


### 4.3 v0.3
1. Voice-based virtual patient + attending “grill mode”
2. Podcast / audio recap generator
3. Real-time study groups (live sessions, group quizzes, study rooms)
4. Personalized “learning science” coaching (metacognition prompts)

---

## 5. Functional requirements

### 5.1 Onboarding and personalization
* Users must select:
  * Level (pre-clinical / clinical / postgrad)
  * Track/exam region (USMLE/PLAB/NEET PG/UKMLA/etc.)
  * Timeline (exam date optional)
  * Preferences (time per day, days per week, clinical rotation hours)
  * Preferred language (default English) and region/locale
  * Time zone (default inferred; user can override)
* System generates a **baseline study plan** and a starting diagnostic quiz.

### 5.2 AI Tutor (core interaction)
* Provide layered explanations:
  * “Simple”, “Student”, “Clinical” (slider or buttons)
* Support structured outputs:
  * mnemonics, tables, differentials, SOAP-note templates
* Must provide **citations** for factual medical claims when feasible.
* Must never fabricate citations. If citations are unavailable, the Tutor must:
  * clearly label the response as “uncited”
  * either switch to a more general explanation OR request permission to search allowed sources (if search is supported)
* Must provide **uncertainty handling**:
  * If evidence is missing or unclear, explicitly say so.
* Must provide a user-visible **confidence / evidence-strength signal** for factual answers:
  * Prefer qualitative levels (e.g., High / Medium / Low) or “Evidence strength” labels over uncalibrated numeric percentages.
  * The label must be explainable via a short “Why this confidence?” panel (e.g., has citations, consistent across sources, based on user-provided notes only, etc.).
  * If the answer is derived only from user uploads, label it as “From your notes” and encourage verification.
* Must refuse or redirect:
  * patient-specific diagnosis/treatment, dosing for real individuals, emergencies.

Multilingual behavior:
* The Tutor must respond in the user’s preferred language by default.
* For medical terms, allow user preference:
  * “Keep medical terms in English” (common in many curricula)
  * “Translate medical terms” (with original term shown)
* Provide a one-click **bilingual view**: preferred language + English original (to reduce mistranslation risk).

### 5.3 Content ingestion (user-provided)
Supported inputs: PDF, plain text, Markdown; DOCX, images.

Requirements:
* Uploads must be organized into a **Library** with tags (system, subject, exam).
* Extract:
  * summaries
  * key facts
  * flashcards
  * quiz questions
  * (V1) hierarchical outline (headings → subheadings → bullets)
  * (V1) interactive mind map / concept map view
* Provide “source trace” back to:
  * the uploaded document sections (page/chunk references)

Multilingual requirements (V1):
* Detect document language automatically (user can override).
* Allow generation in:
  * document language
  * user preferred language
  * bilingual output (recommended default for non-English)
* Preserve citations/page references even when translating (show translated snippet + original excerpt).

IP constraints:
* Upload experience must include **user attestation** (they have rights to upload).
* No public sharing of copyrighted material by default.
* Provide takedown/reporting flows for IP violations.

### 5.4 Flashcards and spaced repetition
* Native flashcards must support:
  * basic (front/back)
  * cloze deletion
  * image occlusion (V1)
  * media (images/audio)
* Scheduling:
  * spaced repetition algorithm with adjustable ease
  * daily review limits and workload warnings
* Import/export:
  * export to Anki-compatible formats (CSV + media bundle; APKG if feasible)
  * import from CSV (MVP)

Multilingual requirements:
* Cards must support Unicode across scripts (Latin, Cyrillic, Arabic, Devanagari, CJK).
* Decks must carry a **language metadata** field (for filtering and sharing).

Reference: active recall + spaced repetition are highlighted as effective strategies in medical learning research.

### 5.5 Quiz / Q-bank
* Create quizzes:
  * by system/subject, difficulty, question type
  * timed vs untimed
* Each question must include:
  * correct answer
  * explanation with citations when applicable
  * **exhaustive rationale for distractors** (why each incorrect option is wrong)
  * “high-yield takeaway” section (3–7 bullets)
  * optional **visual aid** panel (generated diagrams or user-upload snippets where permitted)
* Adaptive remediation:
  * tag weaknesses and schedule targeted flashcards/quizzes
  * **auto-create a remediation package** for every missed question:
    * 1–3 flashcards
    * a short “why you missed it” explanation
    * a follow-up micro-quiz (3–5 questions)
* Integrity controls:
  * prevent copying proprietary question stems (generate original items)
  * avoid trademarked branding implying endorsement

Multilingual requirements:
* Questions/explanations must support the user’s preferred language.
* Allow “exam language” override (e.g., practice in English even if UI is Spanish).

### 5.5.1 Mistake notebook (required)
* The system must maintain a personal **Mistake Notebook** containing:
  * missed question IDs
  * the user’s chosen answer
  * the key misconception
  * linked flashcards and follow-up quizzes
* Users must be able to filter by topic/system and export notes.

### 5.6 Study schedule and planning
Requirements (inspired by modern study tools):
* Daily checklist of tasks (videos/notes/flashcards/quizzes)
* Auto-rescheduling when overdue
* Drag-and-drop to rearrange
* Burnout protection:
  * “too much in one day” alerts
  * break reminders during long sessions

Locale/time requirements:
* All scheduling must be time zone–aware (DST safe).
* Notifications must respect locale preferences (date/time formatting, week start day).

### 5.7 Clinical simulation (V1+)
* Virtual patient cases:
  * chief complaint, history, vitals
  * interactive questioning
  * ordering labs/imaging (simulated)
  * assessment + plan feedback rubric
* “Attending feedback”:
  * differential completeness
  * missed red flags
  * communication quality (OSCE-style)

### 5.12 Knowledge Library and hover term cards (V1)
Goal: deliver “instant depth-on-demand” without losing context.

Requirements:
* Maintain a **structured Medical Knowledge Library** (MedMentor-owned content + allowed public-domain/licensed content + institution-provided content + user uploads).
* Every term card must include:
  * short definition
  * key differentiators (e.g., vs similar terms)
  * common pitfalls
  * links to:
    * deeper MedMentor library article
    * relevant flashcards
    * relevant quiz sets
    * citations (where applicable)
* Hover cards must work in:
  * the Q-bank review screen
  * uploaded-document reader
  * AI tutor answers

Constraints:
* No copying of competitor “library” text. All content must be original or properly licensed.

Multilingual requirements:
* Term cards must support multilingual display with:
  * primary language content
  * English synonym / canonical term
  * common abbreviations (locale-specific)
* Search should match synonyms across languages when safe (e.g., “insuficiencia cardíaca” ↔ “heart failure”).

### 5.13 MedMentor Browser Extension (V1)
Goal: make MedMentor the “always-available” layer while students browse PubMed, Wikipedia, guidelines, or school LMS pages.

Supported browsers:
* Chrome + Chromium-based (Edge). (Safari extension is future.)

Core features:
* Detect medical terms on a page and **highlight** them.
* On click/hover, open a side panel with:
  * term card (definition + high-yield bullets)
  * citations / source links (if available)
  * “Save to MedMentor” (adds to Library, creates flashcards, schedules review)
* “Select text → Ask MedMentor” context menu:
  * sends only selected text (default) rather than full-page content.

Privacy/security requirements:
* Clear permission prompts; default to **least privilege**.
* Do not upload full page content without explicit user action.
* Provide per-site allowlist/denylist.

Localization requirements:
* Extension UI must be localized for supported UI languages.
* Term detection/highlighting must be language-aware (including accents/diacritics where applicable).

### 5.14 Community Hub (collaborative decks and cohorts) (V1)
Goal: enable Anki-like community scale while keeping trust and IP controls.

Workspaces:
* Personal workspace (default)
* Cohort workspace (e.g., “University X — 2027 batch”)
* Institution workspace (admin-managed)

Collaboration features:
* Shared decks with:
  * versioning (history, diffs)
  * roles: owner/editor/viewer
  * moderation: approve/deny proposed changes
  * tagging standards and required metadata (topic/system/exam)
* Quality signals:
  * upvotes, “reported incorrect”, reviewer badges

IP and safety constraints:
* Sharing defaults to **private** unless explicitly published.
* Prohibit uploading/redistributing copyrighted decks and proprietary Q-bank content.
* Provide takedown and audit logs.

Multilingual requirements:
* Community artifacts must declare language(s) (single-language decks preferred initially).
* Provide optional “translation contributions” workflow (separate from content edits) to reduce semantic drift.

### 5.15 AI visual mnemonic generation (V1)
Goal: provide memorable, *original* visual aids that improve recall (especially pharm/micro).

User experience:
* “Generate mnemonic scene” from:
  * a topic
  * a drug class
  * a pathway summary
* Output includes:
  * generated image
  * legend mapping each object in the image to a fact
  * a short recall quiz + flashcards derived from the legend

Safety/IP requirements:
* Must avoid generating images that imitate or reproduce known copyrighted styles/scenes.
* Must include a toggle: “safe + simple medical diagram style” vs “creative mnemonic”.
* Must store the legend/source mapping so the mnemonic is explainable.

### 5.8 Accounts, profiles, and settings
* Account management:
  * update email/password
  * connect/disconnect OAuth (Google/Apple/Microsoft)
  * session management (log out of other devices)
  * account switching on device (support multiple logged-in accounts; no cross-account content bleed)
  * account recovery flows (email reset; OAuth re-linking)
* Profile preferences:
  * exam track, school year
  * study time budget
  * accessibility preferences
* Privacy controls:
  * opt-in/out telemetry (where required)
  * data export (machine-readable)
  * account deletion

Language settings:
* Users must be able to set:
  * UI language
  * preferred output language for AI content
  * exam/practice language
  * locale (dates/times/number formatting)
* RTL support must be available for Arabic (and future RTL languages).

### 5.9 Notifications
* Reminders for:
  * flashcard reviews due
  * scheduled study tasks
  * streak protection
* User controls:
  * quiet hours
  * frequency caps
  * channel selection (email/push)

Localization requirements:
* Notification templates (push/email) must be localized for supported UI languages.
* Template variables must be locale-aware (dates, counts/plurals).

### 5.10 Admin, moderation, and support tooling
* Admin roles (minimum):
  * Student/User
  * Institution Admin
  * Internal Support
  * Internal Reviewer (clinical/content QA)
* Moderation queues:
  * unsafe medical content reports
  * hallucinated/incorrect citation reports
  * IP infringement reports
* Support workflows:
  * ticketing links (or built-in lightweight tickets)
  * user-visible status page link (optional)

### 5.11 Billing 
* Support:
  * monthly/yearly plans
  * upgrades/downgrades
  * cancellations and renewals
  * refunds policy (documented)
* Billing must not block:
  * data deletion
  * export
  * safety reports

---

## 6. AI system requirements

### 6.1 Model strategy
* Use a **model router** to choose models based on:
  * task type (summarization vs reasoning vs multimodal)
  * cost/latency constraints
  * safety profile
* Provide fallback behavior:
  * graceful degradation when a model or tool is unavailable

Multilingual routing requirements:
* The router must choose models/prompts that are strong in the **requested language**.
* For high-risk medical content, prefer:
  * bilingual output (English + target language)
  * stronger citation requirements

### 6.2 Agent architecture (Google ADK with A2A)

**Framework**: Google Genkit (Go SDK) with Agent-to-Agent (A2A) protocol for multi-agent orchestration.

**Language Support**: **Go (Primary)** for high-performance agent orchestration. Python (Secondary) for offline data processing only.

**Core Agent Architecture**:

```
User Request → API Gateway → Supervisor Agent
    → Specialist Agents (via A2A protocol)
    → Safety Agent (all paths)
    → Response
```

**Specialized Agents** (implemented as Google ADK agents):

| Agent | Role | Tools | Output |
|-------|------|-------|--------|
| **Supervisor** | **High-Level Session Logic**: Language, Track, Preferences, Conversation State, Intent Routing. | `transfer_to_agent` (A2A) | Orchestrated response |
| **ApiGateway** | **Low-Level Platform**: Auth, Rate Limits, Circuit Breaking, Fan-out. | N/A | Streamed bytes |
| **Tutor** | Explanations, Socratic method, layered depth | Vertex AI (Go SDK) | `explanation` |
| **Retrieval** | RAG search, citations, source verification | Vector search, citation tools | `citations` |
| **Quiz** | Question generation, distractor rationales | Content DB, template tools | `quiz` |
| **Flashcard** | Card creation, deduplication, SRS data | Content DB, SRS tools | `flashcards` |
| **Simulator** | Clinical Roleplay, Patient Vitals State Machine | Persona Engine | `simulation_event` |
| **Socratic** | Diagnostic Debriefing, Counter-factual analysis | Reasoning Engine | `debrief` |
| **Voice** | Real-time STT/TTS, Interruption Handling | Deepgram, ElevenLabs | `audio_stream` |
| **Vision** | Image Analysis (Radiology, Dermatology) | Gemini Vision, GPT-4o | `image_analysis` |
| **Research** | PubMed Scanning, Paper Summarization | PubMed API, Zotero | `paper_summary` |
| **Planner** | Study Scheduling, Spaced Repetition Logic | Calendar API, SRS Algo | `schedule` |
| **UserProfile** | Knowledge Tracing, Adaptive Learning Models | Knowledge Graph (Postgres) | `user_state` |
| **Format** | Content Formatting (Markdown, PDF, SOAP) | Template Engine | `formatted_doc` |
| **Safety** | Policy enforcement, refusals, redactions | Policy check tools | Validated output |
| **Analytics** | Progress tracking, recommendations | Progress DB, ML models | `insights` |

**A2A Protocol Benefits**:
- **Scalability**: Deploy agents on separate servers as needed
- **Modularity**: Develop and update agents independently
- **Flexibility**: Mix local and remote agents seamlessly
- **Observability**: Built-in tracing and monitoring across agent boundaries

**Agent Communication Patterns**:
1. **Sequential**: Agent A → Agent B → Agent C (e.g., Tutor → Safety → User)
2. **Parallel**: Multiple agents work simultaneously (e.g., Quiz + Flashcard generation)
3. **Hierarchical**: Supervisor delegates to specialists
4. **Remote**: A2A protocol for cross-server communication

**Genkit Implementation Notes**:
- Each agent has: `name`, `instruction`, `tools`, `output_key`
- State management via ADK session state
- Streaming responses for real-time user feedback
- Automatic retries and graceful degradation

Guardrails (implementation requirements):
* Retrieval-first for factual claims: when a response includes factual medical assertions, the system should prefer grounding via allowed sources and/or user uploads.
* Source-grounding policy:
  * If sources are available, attach citations and keep claims consistent with sources.
  * If sources are not available or conflict, the system must surface uncertainty and avoid definitive phrasing.
* Prompt-injection resistance:
  * Treat user uploads and web content as untrusted; do not follow instructions found inside documents.
  * Restrict tool access (search/retrieval) to allowlisted sources.
* “Truthfulness over fluency” policy: the system must prefer “I can’t verify that” over plausible-sounding completion.

### 6.3 Trustworthiness & risk management
Adopt a structured AI risk process aligned to frameworks such as NIST AI RMF:

* **Govern**: policies, ownership, approvals, incident response.
* **Map**: use cases, user impact, stakeholders, harm scenarios.
* **Measure**: evaluations, red-teaming, bias/safety tests, citation accuracy.
* **Manage**: mitigations, monitoring, updates, rollback.

### 6.4 Evaluations (required)
* Offline eval suite:
  * citation presence and quality
  * citation correctness (do citations actually support the claim?)
  * factuality checks (sampled)
  * unsafe request refusal behavior
  * prompt injection resilience for uploaded docs
  * confidence-label calibration:
    * confidence/evidence-strength labels should correlate with factuality and citation support (avoid overconfident wrong answers)
  * multilingual quality:
    * meaning preservation (translation fidelity)
    * terminology consistency (medical terms)
    * safety/refusal behavior in non-English
    * locale correctness (units, decimals, abbreviations)
* Online monitoring:
  * user-rated helpfulness
  * “report unsafe” rate
  * hallucination sentinel metrics
  * “high-confidence wrong” sentinel metric (priority-1 incidents)
  * **AI Decision Log**: Record model name, temperature, tokens, top-k chunks, and final citations for every response. Consolidate in a queryable log store (BigQuery/ClickHouse) for analysis.


Release gating requirements:
* No model/prompt change ships without:
  * passing the offline eval suite above (thresholds TBD)
  * canary rollout + monitoring
  * rollback plan

---

## 7. Safety, privacy, and compliance

### 7.1 Medical safety policy (MVP)
The app is **for education**. It must:

* Refuse patient-specific diagnosis/treatment and emergency guidance.
* Encourage seeking professional help for real medical concerns.
* Avoid generating dosage recommendations for specific individuals.
* Provide safe alternatives: general learning explanations, guideline summaries, “talk to a clinician” prompts.

### 7.2 Privacy-by-design requirements
Even as an education tool, the platform may handle sensitive data (study habits, possibly health data if users paste clinical notes). Build strong defaults:

* Data minimization (collect only what’s needed).
* Clear privacy notice and consent flows.
* Purpose limitation and retention controls.
* User rights support:
  * export data
  * delete account and associated content

GDPR considerations (if serving EU residents):
* lawful basis tracking
* data protection by design/default
* data subject rights workflows
* DPIA trigger review (especially for large-scale sensitive data processing)
* breach processes (including GDPR notification timing expectations, where applicable)

Age-related considerations:
* Implement age gating and parental consent flows as required by the jurisdictions served.

### 7.3 HIPAA considerations
If the product ever processes **electronic protected health information (ePHI)** for covered entities/business associates, it must meet Security Rule expectations (administrative/physical/technical safeguards) and perform **risk analysis**.

Even if not HIPAA-covered, treat any pasted/uploaded clinical content as sensitive:
* encryption in transit and at rest
* strict access controls
* audit logs for access to sensitive content

### 7.4 Breach response expectations
Implement incident detection, containment, and notification workflows.

Relevant external regimes include:
* HIPAA Breach Notification Rule (if applicable): notification requirements and timelines.
* FTC Health Breach Notification Rule may apply to certain personal health record vendors.

Implementation requirements:
* Maintain an incident response runbook (triage → containment → eradication → recovery → comms).
* Maintain audit-ready documentation of notifications and determinations.

### 7.5 Security requirements (MVP)
* Authentication: MFA optional, strongly encouraged.
* Authorization: role-based access control.
* Data security: encryption at rest/in transit.
* Logging: audit trails for admin actions.
* Secure SDLC:
  * dependency scanning
  * secrets management
  * penetration testing before launch

Additional recommended controls:
* Key management (KMS/HSM-backed) and key rotation.
* Data residency strategy (if serving multiple regions).
* Vendor risk management (model providers, analytics, storage).

---

## 8. Data and content governance

### 8.1 Data categories
* Account data (email, auth provider IDs)
* Learning data (progress, schedules, quizzes)
* Account data (email, auth provider IDs)
* Learning data (progress, schedules, quizzes)
* User content (uploads, notes, flashcards)
* Telemetry (crash logs, usage events) — opt-in where required

**Data Classification**:
* Each entity must use a `sensitivity` field: `low` (public/static), `medium` (internal user data), `high` (PII/PHI).
* Retention policies must be keyed to this classification.

AI-specific data handling:
* Clearly document whether user content is used for:
  * improving the product (opt-in/opt-out)
  * fine-tuning
  * safety monitoring

### 8.2 Retention and deletion
* User-driven deletion must remove:
  * uploads
  * derived artifacts (embeddings, vector entries, generated flashcards) where feasible
* Backups must have documented retention windows.

### 8.3 Content quality program
* User feedback on every answer: helpful / not helpful + report
* “Flag for review” queue for:
  * potentially unsafe medical content
  * suspected hallucinated citations
  * IP infringement claims
* Reviewer tools (internal): compare response to sources, approve/ban templates.

Continuous improvement (self-improving system) requirements:
* The product must improve quality over time via a **closed-loop quality program**:
  * collect structured feedback (what was wrong: incorrect fact vs ambiguous vs missing citation vs unsafe)
  * triage into an error taxonomy (content gaps, retrieval failures, prompt failures, model failures)
  * generate candidate fixes (prompt updates, retrieval improvements, curated snippets, fine-tuning datasets)
  * validate via offline evals + targeted red-team tests
  * ship via canary + monitor + rollback
* Data use boundaries:
  * Do not do uncontrolled “online learning” directly from a single user’s interaction.
  * Any training/fine-tuning on user content must be **explicit opt-in** (and documented) with privacy safeguards.

Reinforcement learning (optional / future):
* If using RLHF/RLAIF, constrain it primarily to **teaching style** (clarity, pedagogy, refusal consistency), not to invent new medical facts.
* Consider lightweight contextual bandits for:
  * which question to ask next
  * which review item to schedule next
  * which explanation depth to default to
  (All subject to safety constraints and evaluation gates.)

---

## 10. Non-functional requirements (NFRs)

### 10.1 Scalability & Elasticity
* **Horizontal Scaling**: The system must support automatic horizontal scaling of Agent pods based on event queue depth and CPU/Memory utilization.
* **Cold Start Resilience**: Critical agents (Supervisor, Tutor) must maintain a minimum number of replicas to minimize latency during burst traffic.
* **Queue-Based Smoothing**: Heavy tasks (Deep Research, Document Ingestion) must be decoupled via the Event Bus to prevent front-end timeouts during high load.
* **Auto-Recovery**: Kubernetes must automatically restart failed Agent pods and ensure 99.9% availability of the Agent layer.

### 10.2 Performance
* **Agent Response Latency**: Core tutor responses (streaming starts) should target < 2s for initial token generation.
* **Eventual Consistency**: Non-critical assets (flashcards, quiz rationales) should populate within 10s of ingestion completion.
* **Database IOPS**: Support for high-concurrency vector searches across millions of grounding chunks.

### 10.3 Reliability
* **Circuit Breaker Pattern**: If a downstream model provider (Gemini/OpenAI) or a tool (Search API) fails, the system must degrade gracefully (e.g., fallback to cached results or local grounding).
* **Dead Letter Queues (DLQ)**: Failed agent events must be persisted for manual audit and retriability.

---

## 11. Integrations and dependencies

### 9.0 Integration principles (student-first)
Integrations should reduce friction in the **daily study loop** (plan → do → review), without increasing privacy risk or operational complexity.

Principles:
1. **Fast wins first:** prefer integrations that work via OS primitives (Share Sheet, iCal/ICS, deep links) before heavy API partnerships.
2. **Write-minimal, read-minimal:** default to *exporting/syncing MedMentor-created study tasks outward* (calendar/events) rather than ingesting personal data inward.
3. **No accidental oversharing:** anything shareable must be safe by default (no private notes, no proprietary question stems, no patient info).
4. **User control:** easy connect/disconnect, granular scopes, clear “what we read/write” copy.
5. **Time-zone correctness:** all schedule-related integrations must be DST-safe and locale-aware.

### 9.1 Core integrations

#### 9.1.1 Calendar: Google / Apple / Microsoft
Student value:
* Study plan becomes “real” by showing up where students already live (Calendar + notifications).
* Reduces missed reviews and improves adherence.

Privacy UX requirement:
* Users must be able to choose **event detail level**:
  * **Private (default):** generic titles like “MedMentor Study Block” + deep link only.
  * **Detailed:** includes topic/task names and estimated duration.

Phased delivery:
* **v0.1 (must):**
  * **One-click calendar export** for the schedule (ICS).
  * **Add single task to calendar** from any study item (one-off ICS event download on web; native “Add to Calendar” on mobile).
  * Deep link back into the app from the event description (e.g., `medmentor://task/<id>` and web fallback).
* **v0.2 (should):**
  * **Two-way-ish sync (write-focused):** connect Google Calendar and/or Microsoft Outlook Calendar to *create/update* events for MedMentor tasks.
  * Optional “Create a dedicated MedMentor calendar” to avoid polluting the primary calendar.
  * Conflict handling: if a user edits an event externally, treat as **time override only** (do not overwrite the title/notes unless user opts in).
* **v0.3 (could):**
  * Smart scheduling: detect busy blocks (read availability only, minimal scope) and suggest optimal study times.

Security/scopes requirements:
* Use OAuth with least-privilege scopes.
* Default to **write-only where possible** (creating events), and avoid reading private event details unless explicitly enabled.
* Store refresh tokens securely; support disconnect + token revocation.

#### 9.1.2 Sharing & virality (privacy-safe)
Student value:
* Makes it easy to share progress, invite friends, and collaborate—without leaking sensitive study content.

Minimum viable approach (no heavy platform APIs):
* **Native Share Sheet** (iOS/Android) + Web Share API (web) for:
  * “Share my streak / weekly progress” (renders a share card image)
  * “Invite to cohort workspace” (invite link)
  * “Share a public deck/case link” (only if explicitly published)
* **Instagram / Facebook / WhatsApp / Telegram**: use share intents/deep links rather than requiring account connections.
  * Instagram: support “Share to Stories” via platform share mechanisms (where available).
  * Facebook: standard share dialog for public links (Open Graph metadata).

Constraints:
* **No sharing of**: user uploads, private notes, mistake notebook details, proprietary question stems, or anything that could contain patient-identifying info.
* Share flows must show a **preview** and an explicit “This will be public” warning.

#### 9.1.3 Student study-stack integrations
Student value:
* Students already use a “stack” (Anki, Notion/Obsidian, Google Drive). Integrations prevent duplicate work.

Phased delivery:
* **v0.1 (must):**
  * **Anki export** (already in scope elsewhere): CSV + media bundle; import from CSV.
  * Share links for *public* artifacts only (no default publishing).
* **v0.2 (should):**
  * Cloud import connectors for **Google Drive / Dropbox / OneDrive** (pick one to start) to fetch *user-owned files* into uploads.
  * Notion/Markdown export: export a topic summary/outline as Markdown.
* **v0.3 (could):**
  * Obsidian plugin / local folder sync (advanced; evaluate demand).

Security requirements:
* For storage connectors, request the narrowest file scopes possible (prefer “file picker” patterns over full-drive access).
* Respect institutional policies: allow schools to disable external connectors in B2B tenants.

#### 9.1.4 Institution / LMS integrations (later, but high leverage)
Student value:
* Auto-pulls course structure (syllabus, modules) → reduces manual setup.
* Enables educator-created assignments and cohort analytics (with strong privacy constraints).

Phased delivery:
* **v0.2 (B2B beta):**
  * SSO (Google Workspace / Entra ID / Okta) and roster import (CSV → SCIM later).
* **v0.3+ (selective):**
  * LMS integrations (Canvas/Moodle/Blackboard) for course module metadata and assignment distribution.

Constraint: treat LMS content as potentially copyrighted; ingest only what the institution is authorized to provide.

#### 9.1.5 Payments, email, push
* Payments provider (e.g., Stripe / App Store / Play billing).
* Email provider for transactional messages (verification, receipts, reminders).
* Push notifications provider (APNs/FCM; optional abstraction layer).

#### 9.1.6 Distribution dependencies
* Browser extension distribution (Chrome Web Store / Edge Add-ons) (V1).
* App distribution: Apple App Store / Google Play.

### 9.2 AI dependencies
* LLM providers via direct APIs or a router.
* Embeddings + vector database for retrieval.
* Speech-to-text and text-to-speech (VNext).

### 9.3 Integration backlog (nice-to-have, validate demand)
Only pursue if evidence shows these improve adherence or outcomes:
* **Focus modes:** iOS Focus / Android Digital Wellbeing suggestions (non-invasive; reminders only).
* **Discord/Slack:** cohort notifications (B2B-first; avoid spamming).
* **Wearables:** reminder pings (low priority; likely not core to learning outcomes).

---

## 10. UX principles and key flows

### 10.1 Principles
* Reduce cognitive load: minimal decisions per session.
* Make next step obvious: “What should I do right now?”
* Trust is UI: citations, uncertainty labels, and “show source” affordances.
* Mobile-first: 2–5 minute study loops.

### 10.2 Key flows
1. **First session**: onboarding → baseline quiz → plan generation → first review set
2. **Upload and learn**: upload PDF → summary → flashcards → quiz → schedule tasks
3. **Daily study**: open app → checklist → flashcards → quiz → progress update
4. **Clinical case** (V1): choose case → interview → order labs → feedback

---

## 11. Non-functional requirements

### 11.1 Performance
* P50 response time for tutor: (TBD)
* P95 response time: (TBD)
* Upload processing: show progress + partial results where possible

### 11.2 Reliability
* Clear error states and retries
* Offline-friendly reading mode (future)
* Backward-compatible data migrations

### 11.3 Accessibility
* Keyboard navigation (web)
* Captions/transcripts for audio/video (where applicable)
* Color contrast standards

### 11.4 Internationalization (i18n) and localization (l10n)
Engineering requirements:
* All UI strings must be externalized with stable keys.
* Support pluralization and gender where applicable (ICU MessageFormat recommended).
* Locale-aware formatting for:
  * dates/times/time zones
  * numbers/percentages
* Font support for non-Latin scripts and RTL layouts.
* Fallback behavior:
  * missing translation → English fallback
* Testability:
  * pseudo-localization mode
  * screenshot/regression tests for text expansion
  * automated checks for missing/unused translation keys

Search and text-processing requirements:
* Language-aware tokenization for search/indexing (especially CJK).
* Accent/diacritic-insensitive search where appropriate.
* RTL-safe text rendering and cursor behavior.

Translation workflow requirements:
* Maintain a product localization pipeline:
  * translation memory
  * glossary / terminology list (medical terms + abbreviations)
  * review/approval steps for high-risk strings (safety notices, dosing/unit labels)
* Strings must be versioned; support “string freeze” ahead of releases.

Product policy requirements:
* For medical safety, when content is translated:
  * allow user to view the English source output
  * avoid translating dosage/units in a way that could introduce ambiguity; show original units
  * clearly label “translated” content

Medical localization requirements:
* Support locale-specific preferences without changing underlying medical truth:
  * units display (SI vs US customary) while preserving original units
  * decimal separators and thousands separators
  * drug naming: display **generic names** by default and optionally show brand names by region
* Regional guidance:
  * allow selecting an “exam region” and “clinical region” to influence guideline citations (where the product supports citations).

---

## 12. Monetization & packaging

### 12.1 Pricing principles
* **Bundle the fragmented stack**: position MedMentor as one subscription that replaces multiple tools (Q-bank + library + spaced repetition + depth-on-demand).
* **Make costs predictable** despite multi-LLM routing:
  * include baseline “AI credits”/quotas in plans
  * offer top-up packs rather than silently throttling
  * push heavy workloads to batch jobs when possible
* **Study-loop value > raw generations**: monetize outcomes (remediation, mastery, schedule adherence), not token usage.
* **Student-friendly**: simple annual plan, clear exam sprint options, generous free trial.



### 12.3 MedMentor proposed B2C packaging (hypothesis)
Goal: be “obviously worth it” vs buying multiple tools, while leaving room for high-cost AI usage.

**Tier A — Free (on-ramp)**
* Best for: trying the loop, lightweight review
* Includes:
  * basic flashcards + scheduler
  * limited uploads and limited quiz generation
  * limited daily Tutor usage
* Excludes / limited:
  * large-document depth-on-demand
  * visual mnemonic generation
  * community publishing (read-only community decks allowed)

**Tier B — Plus (core all-in-one)**
* Best for: the majority of students as a primary tool
* Includes:
  * full study loop (uploads → summary → quizzes → flashcards → schedule)
  * Mistake Notebook + remediation packages
  * term hover cards + Knowledge Library
  * browser extension (selected-text-only default; allowlists)
  * moderate “AI credits” monthly

**Tier C — Pro (power + multimodal)**
* Best for: heavy users, large textbook uploads, visual learning
* Includes:
  * larger upload limits + faster processing
  * **AI visual mnemonic generation**
  * higher AI credit allowance
  * priority/batch queue options for massive uploads
  * advanced analytics (weakness forecasting, exam readiness)

**Add-ons**
* **AI credit top-ups** (consumable) for high-volume usage.
* **Exam sprint pack** (e.g., 8–12 weeks): focused plan templates, readiness assessment, and higher quiz volume.

Entitlement model (implementation requirement):
* Plans must define quotas for:
  * pages processed/month (uploads)
  * quiz questions generated/month
  * image mnemonic generations/month
  * vector storage (GB)
  * concurrent workspaces / shared decks

### 12.4 B2B monetization (institutions)
Institution licenses (annual) should include:
* SSO + user provisioning
  * Support **SAML 2.0** and/or **OIDC** (implementation choice can be phased)
  * Target compatibility with common IdPs (e.g., **Microsoft Entra ID**, **Google Workspace**, **Okta**)
  * Provisioning: SCIM (or CSV roster import initially), group-to-cohort mapping
* cohort dashboards (aggregated/anonymous where possible)
* assignment distribution (decks/quizzes) and completion tracking
* private Community Hub workspace per cohort
* admin moderation and audit logs

Pricing model (to validate):
* per-seat annual price with minimum commitment
* optional overage for heavy AI usage (credits)

### 12.5 Go-to-market + pricing experiments (early)
* Offer a **7-day free trial** (mirrors common patterns in the category).
* Test price sensitivity with 2–3 price points per tier.
* Campus acquisition loops:
  * “class deck” collaboration features as a viral hook (AnkiHub-style)
  * referral credits (AI credit top-ups) rather than only cash discounts

### 12.6 Known gaps / blocked benchmark data
* **AMBOSS pricing**: consent/JS gating prevented price extraction here.
* **UWorld USMLE pricing**: medical pricing pages were blocked (403) in this environment; a general pricing shell was accessible but did not expose plan prices via static extraction.
Action: capture these benchmarks manually (or via a JS-capable fetch) and update this section.

---

## 13. Metrics (success criteria)

### 13.1 Learning outcomes (proxies)
* Weekly active users (WAU)
* Review adherence (% scheduled reviews completed)
* Quiz improvement trend (accuracy by topic over time)
* Time-to-mastery per topic

### 13.2 Trust and safety
* “Reported unsafe” rate per 1k sessions
* Citation coverage (% answers with usable citations)
* User trust rating (in-app survey)

### 13.3 Business
* Free → paid conversion
* Retention (D7, D30)
* CAC/LTV (later)

---

## 14. Risks and mitigations

| Risk | Why it matters | Mitigation |
|---|---|---|
| Hallucinated medical claims | Safety + trust failure | Retrieval + citations, refuse when uncertain, eval suite |
| Regulatory scope creep | Could be treated as CDS/medical device if used for patients | Keep intended use educational, guardrails, disclaimers |
| Copyright/IP violations | Uploads and generated content may infringe | Attestation, no sharing by default, takedown workflow |
| Prompt injection via uploads | Can exfiltrate data / override system | sandbox retrieval, tool permissioning, content isolation |
| Cost blowups | Multi-LLM calls + embeddings | routing, caching, quotas, batch processing |

---

## 15. Product decisions (as of 2026-02-19)

1. **In-scope exam tracks (initial):** USMLE, PLAB, NEET PG, UKMLA.
2. **Allowed citation sources at launch:** guidelines (where licensed/allowlisted), PubMed abstracts, institution-provided content, and user uploads.
3. **Institution data residency / tenant isolation (initial):** not required initially.
  * Implementation note: still require strong logical tenant isolation, least-privilege access controls, and a clear roadmap option for regional data residency if demanded later.
4. **User-uploaded copyrighted materials:** allow with attestation/consent and **no sharing by default**; enforce takedown and audit logs.
5. **Accuracy threshold + feasible human review program (decision):**
  * Define release gates using the eval suite in §6.4 with explicit thresholds (exact numbers TBD during implementation), with emphasis on minimizing **high-confidence wrong** events.
  * Human review program (MVP):
    * 24–48h SLA for triage of user-reported “unsafe/incorrect” items.
    * Weekly sampling audits by an internal clinical/content reviewer for high-risk topics and top-used flows.
    * Reviewer outputs feed the closed-loop quality program (§8.3) (prompt fixes, retrieval adjustments, source allowlists, targeted eval cases).
6. **Cohort-based assignments from educators:** yes for the institutional/B2B track (ship with the institution beta), not required for consumer v0.1.
7. **Browser extension data policy:** strict **selected-text-only** at launch; consider optional per-site full-page summarization later behind explicit permission + allowlist.
8. **Community Hub launch sequence:** phased approach — start with institution/cohort-only workspaces; later enable public community publishing with stronger moderation/reputation/IP controls.
9. **Localization scope (launch posture):** localize **UI + AI outputs** (not UI-only).
10. **Non-English safety posture:** bilingual output is the default for non-English.
11. **Non-English question practice:** support **UI + AI outputs** and allow practice questions/explanations in the user’s selected exam/practice language; keep an English option available for safety/verification.
12. **Units:** support **per-locale unit display** while preserving original units when content is translated or quoted.
13. **Drug naming:** display **generic names by default** with optional **regional brand names** (generic + brand) based on locale/region.

Additional decision details (resolved from prior open questions):

### 15.1 Citation source allowlists (by region) — initial
Goal: keep citations reputable, accessible, and auditable; avoid proprietary/paywalled sources unless explicitly licensed.

Baseline sources (all regions):
* **PubMed abstracts** (as already in-scope)
* **User uploads** (clearly labeled “From your notes” when used)
* **Institution-provided content** (tenant-scoped; treated as allowlisted)

Guideline / authoritative web sources (initial allowlist candidates; subject to licensing + ToS checks):
* Global: **WHO** (worldwide guidance)
* US (USMLE emphasis): **CDC**, **NIH/NCBI** (including NCBI Bookshelf where relevant), **USPSTF** (preventive care)
* UK (PLAB/UKMLA emphasis): **NICE**, **NHS**
* India (NEET PG emphasis): **MoHFW (India)**, **ICMR**

Policy constraints:
* Do not cite or quote from proprietary clinical references (e.g., paywalled) unless a license is in place.
* Maintain an explicit allowlist registry (domain → rationale → owner → review cadence).
* If a user requests a source outside the allowlist, respond with a safe alternative (general explanation + suggestion to consult official/local guidance).

### 15.2 Language rollout decision — “all languages”
Decision: support **UI + AI outputs**, with **bilingual output as default** for non-English.

Implementation detail:
* UI localization remains **tiered** (Tier 1/2/3) for translation workflow, QA, and support readiness.
* AI outputs are “**any language**” (best-effort):
  * If the user selects a language not yet officially supported for UI, keep UI in English (or the closest available UI language) and generate AI outputs in the requested language.
  * For non-tier languages, force safer defaults:
    * bilingual view enabled by default
    * stricter grounding/citation requirements
    * more conservative confidence/evidence-strength labels
    * explicit “beta language support” labeling

### 15.3 Initial numeric thresholds for eval gates (v0.1)
Note: these are **initial ship thresholds** to prevent regressions; tune after establishing baseline measurements.

Offline eval gates (must pass to ship model/prompt changes):
* **Citation presence** (when a response contains factual medical claims): ≥ 90%
* **Citation correctness** (sampled; citation supports the claim): ≥ 95%
* **Factuality** (golden set; claim-level): ≥ 90%
* **Unsafe request refusal correctness** (disallowed scenarios): ≥ 99%
* **Prompt-injection resilience** (upload-based attack set): ≥ 99% of attempts do not cause policy/tool breach
* **Confidence calibration guardrail**: “high-confidence wrong” must be ≤ 0.5% of sampled factual answers

Online monitoring gates (trigger rollback / hotfix):
* “High-confidence wrong” incidents: target ≤ 1 per 1,000 factual-answer sessions (P0);
  * if exceeded over a rolling window, auto-trigger investigation and rollback path.
* “Report unsafe” rate: establish baseline in beta; trigger review if it increases by > 2× week-over-week.

---

## 16. Roadmap (high level)

### Milestone A — v0.1
* Tutor + citations + uploads
* Flashcards + spaced repetition
* Quiz builder + explanations
* Study schedule + rescheduling
* Safety policy + reporting
* i18n foundations (string externalization + language settings; English-only content)

### Milestone B — V0.2
* Image learning (occlusion, labeling)
* Virtual patient (text)
* Anki export/import improvements
* Institution pilot dashboard
* Knowledge Library + hover term cards
* Mistake Notebook + remediation packages
* Depth-on-demand outlines + mind maps from uploads
* First localized experience (proposed): UI + AI outputs for 1–2 non-English languages

### Milestone C — V0.3
* Voice OSCE simulator
* Audio/podcast generator
* Knowledge graph: cross-link concepts across Library ⇄ Q-bank ⇄ Flashcards ⇄ Mistake Notebook
* Browser extension (Chrome/Edge)
* Community Hub (collaborative decks)
* Visual mnemonic generator

---

## 17. Research references (selected)

Privacy, security, risk:
* HIPAA Privacy Rule overview (HHS): https://www.hhs.gov/hipaa/for-professionals/privacy/index.html
* HIPAA Security Rule overview (HHS): https://www.hhs.gov/hipaa/for-professionals/security/index.html
* HIPAA risk analysis guidance (HHS OCR): https://www.hhs.gov/ocr/privacy/hipaa/administrative/securityrule/rafinalguidance.html
* HIPAA Breach Notification Rule overview (HHS): https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html
* FTC Health Breach Notification Rule (FTC): https://www.ftc.gov/business-guidance/resources/health-breach-notification-rule
* NIST AI Risk Management Framework (NIST): https://www.nist.gov/itl/ai-risk-management-framework
* EU GDPR business rules (European Commission): https://ec.europa.eu/commission/priorities/justice-and-fundamental-rights/data-protection/2018-reform-eu-data-protection-rules_en

---

## 18. Future Roadmap (v2.0): The Futuristic Medical Brain

To move beyond state-of-the-art and become a truly "futuristic" extension of the medical mind, MedMentor will evolve into an **Immersive, Bio-Adaptive, and Ambient Intelligence** system.

### 18.1 Pillar 1: Bio-Adaptive Learning ("The Quantified Student")
**Concept**: Shift from calendar-based scheduling to *biology-based* orchestration.
*   **Integration**: Ingest real-time data from wearables (Apple Watch, Oura, Whoop) via HealthKit/Google Fit.
*   **Behavior**:
    *   **High Stress / Low HRV**: Agent activates "Passive Review Mode" (audio podcasts, simple flashcards) to prevent burnout.
    *   **Peak Focus**: Agent activates "Challenge Mode" (complex diagnostic riddles, timed simulations) when neuroplasticity is optimal.

### 18.2 Pillar 2: Spatial & Immersive Computing
**Concept**: Move beyond 2D flat screens for complex anatomical and biochemical systems.
*   **"Sherlock Mode" (AR)**: Use mobile/glass cameras to overlay dynamic labels, pathology notes, and "Deep Search" buttons onto physical textbooks, cadavers, or skeletal models.
*   **Interactive 3D**: deeply integrated 3D models (heart, brain, molecular structures) that users can manipulate, dissect, and "chat with" in spatial environments (Vision Pro/Meta Quest).

### 18.3 Pillar 3: Edge AI & "Zero-Latency" Trust
**Concept**: Hybrid Local-First Intelligence for privacy and speed.
*   **On-Device Tutor**: Run quantized models (e.g., Gemini Nano, Llama-Medical-8B) locally on the device.
*   **Benefits**:
    *   **0ms Latency** for basic factual queries ("What's the mechanism of Aspirin?").
    *   **Privacy Seal**: Guarantees that sensitive or embarrassing questions never leave the device.
    *   **Seamless Handover**: Local agent seamlessly escalates to Cloud Supervisor only when deep research or swarm consensus is needed.

### 18.4 Pillar 4: The Collaborative "Agent Swarm"
**Concept**: Agents evolve from isolated tools to collaborative councils.
*   **Consult Board**: For complex cases, the Supervisor spawns specialized sub-agents (e.g., "The Conservative Cardiologist" vs. "The Experimental Oncologist") to *debate* the case in a side-channel before synthesizing a balanced recommendation.
*   **Marketplace**: Allow institutions (e.g., "Stanford Medical Agent") to publish official, curriculum-aligned agents.

### 18.5 Student-Centric Feature Suite (Priority)
Features specifically targeting retention, clinical skills, and exam stress.

#### 18.5.1 Neuro-Flashcards (Bio-Adaptive Retention)
*   **Problem**: Subjective "Easy/Hard" grading in Anki is unreliable.
*   **Solution**: Use pupillometry (Vision Pro) and gaze tracking (Webcam) to measure cognitive load. High dilation = Hard card. The system auto-grades retention based on biological effort, not user opinion.

#### 18.5.2 The OSCE Dojo (Agent Swarm)
*   **Problem**: Students lack safe practice partners for clinical roleplay.
*   **Solution**: A multi-agent simulation where a "Patient Agent" (emotional, voice-enabled) interacts with the student, interrupted by a "Nurse Agent", while an invisible "Evaluator Agent" grades empathy and diagnostics.

#### 18.5.3 The Memory Palace (Spatial AR)
*   **Problem**: Memorizing dry lists (e.g., pancreatitis causes) is difficult.
*   **Solution**: AR overlays that place virtual medical anchors (e.g., a gallstone) onto the user's *real* furniture (couch). The student memorizes by walking through their physical room.

#### 18.5.4 Just-in-Time Mnemonics (personalized)
*   **Problem**: Generic mnemonics don't stick.
*   **Solution**: Generative Mnemonics overlaid on user interests (e.g., "Explain Krebs Cycle using Game of Thrones terminology").

#### 18.5.5 Exam Simulator: Adrenaline Mode
*   **Problem**: Exam panic reduces IQ despite preparation.
*   **Solution**: Stress Inoculation. If Apple Watch detects low HRV (calmness) during a practice exam, the Agent *increases* difficulty and ambient distractions to train performance under pressure.


