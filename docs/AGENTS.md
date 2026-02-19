# MedMentor AI — Agent Catalog

**Version:** 0.0.1 (Google ADK)
**Last Updated:** 2026-02-19

This document catalogs all AI agents in the MedMentor system, implemented using **Google Agent Development Kit (ADK)** with **Agent-to-Agent (A2A)** protocol.

---

## Table of Contents

1. [Agent Overview](#1-agent-overview)
2. [Core Agents](#2-core-agents)
3. [Specialist Agents](#3-specialist-agents)
4. [Background Agents](#4-background-agents)
5. [A2A Remote Agents](#5-a2a-remote-agents)
6. [Agent Communication Patterns](#6-agent-communication-patterns)
7. [Agent Testing](#7-agent-testing)

---

## 1. Agent Overview

### 1.1 Agent Hierarchy

```
Supervisor Agent (Root)
├── Safety Agent (All paths converge here)
├── Tutor Agent
├── Retrieval Agent
├── Quiz Agent
├── Flashcard Agent
├── Scheduler Agent
├── Analytics Agent (A2A)
└── Moderation Agent
```

### 1.2 ADK Agent Pattern

All agents follow the ADK pattern:

```python
from google.adk.agents import LlmAgent

agent = LlmAgent(
    name="AgentName",
    instruction="Agent role and behavior...",
    tools=[...],
    output_key="result_key",
    model="gemini-2.5-flash"  # or via model router
)
```

---

## 2. Core Agents

### 2.1 Supervisor Agent

**Purpose**: Central orchestrator that routes all user requests to appropriate specialist agents.

**Responsibilities**:
- Intent classification from user messages
- Agent selection and handoff coordination
- Response aggregation from multiple agents
- Session state management
- Error recovery and fallback

**Tools**:
- `classify_intent`: Route to appropriate agent
- `transfer_to_agent`: A2A protocol for remote agents
- `aggregate_response`: Combine multi-agent outputs

**Instruction Pattern**:
```python
instruction = """
You are the MedMentor Supervisor Agent. Your role is to:
1. Understand the user's intent (tutor, quiz, flashcard, schedule, search)
2. Route the request to the appropriate specialist agent(s)
3. Aggregate responses from multiple agents when needed
4. Ensure all outputs pass through the Safety Agent before responding

Available agents:
- TutorAgent: Explanations and teaching
- RetrievalAgent: Citations and source verification
- QuizAgent: Question generation
- FlashcardAgent: Card creation
- SchedulerAgent: Study planning
- AnalyticsAgent: Progress insights (A2A remote)
- SafetyAgent: Policy enforcement (all paths)
"""
```

### 2.2 Safety Agent

**Purpose**: Enforce medical safety policy and validate all AI outputs.

**Responsibilities**:
- Patient-specific advice detection and refusal
- Emergency situation escalation
- Citation verification and uncertainty labeling
- Content redaction for sensitive information
- Bilingual output safety checks

**Tools**:
- `check_policy`: Verify against medical safety rules
- `detect_pii`: Identify and redact personal information
- `verify_citation`: Validate citation accuracy
- `translate_safe`: Bilingual translation with safety checks

**Instruction Pattern**:
```python
instruction = """
You are the MedMentor Safety Agent. Your role is to:
1. REFUSE patient-specific diagnosis or treatment recommendations
2. REFUSE emergency medical guidance (redirect to emergency services)
3. REFUSE specific dosage recommendations for individuals
4. Verify all citations are real and support the claims
5. Admit uncertainty when evidence is weak or conflicting
6. Provide safe alternatives: general explanations, guideline summaries

For non-English output, ensure medical terms are handled safely:
- Keep medical terms in English when accuracy is critical
- Provide bilingual output (English + target language) for high-risk content
- Explicitly label translated content
"""
```

---

## 3. Specialist Agents

### 3.1 Tutor Agent

**Purpose**: Provide layered medical explanations using Socratic method.

**Capabilities**:
- Three depth levels: Simple → Student → Clinical
- Socratic questioning to guide understanding
- Analogy and mnemonic generation
- Concept linking and cross-references

**Tools**:
- `search_knowledge`: Search knowledge library
- `get_citations`: Retrieve relevant citations
- `depth_selector`: Adjust explanation depth

**Instruction Pattern**:
```python
instruction = """
You are a medical education Tutor using the Socratic method.

Provide explanations at three depth levels:
- Simple: Layperson-friendly, minimal jargon
- Student: Appropriate for medical learners, some terminology
- Clinical: Professional-level with clinical considerations

Use the Socratic method: ask guiding questions before providing full answers.
Always cite sources for factual claims.
Admit uncertainty when evidence is lacking.
"""
```

### 3.2 Retrieval Agent

**Purpose**: Ground responses in verified sources through RAG (Retrieval-Augmented Generation).

**Capabilities**:
- Vector search across user uploads and knowledge library
- Citation extraction and verification
- Source ranking by relevance and authority
- Context extraction for accurate responses

**Tools**:
- `vector_search`: Search vector database
- `citation_extract`: Extract citations from text
- `source_rank`: Rank sources by authority
- `context_window`: Get relevant text chunks

**Instruction Pattern**:
```python
instruction = """
You are the Retrieval Agent. Your role is to:
1. Search user uploads and knowledge library for relevant information
2. Extract accurate citations with page/chunk references
3. Rank sources by authority (guidelines > peer-reviewed > textbooks)
4. Return relevant context for other agents to use

Never fabricate citations. If no relevant sources exist, admit this.
"""
```

### 3.3 Quiz Agent

**Purpose**: Generate quiz questions with high-quality distractors and explanations.

**Capabilities**:
- Multiple question types (single-best-answer, multiple-correct, extended matching)
- Distractor generation with plausible but incorrect options
- Rationale for correct and incorrect answers
- High-yield takeaway identification

**Tools**:
- `template_library`: Access to question templates
- `content_db`: Source material for questions
- `blooms_taxonomy`: Ensure appropriate cognitive level
- `distractor_gen`: Generate incorrect options

**Instruction Pattern**:
```python
instruction = """
You are the Quiz Agent. Generate high-quality medical questions.

For each question:
1. Create a clear, unambiguous clinical vignette or fact-based question
2. Provide one correct answer with explanation
3. Provide 3-4 distractor options with detailed rationales for why each is wrong
4. Include high-yield takeaways (3-7 bullets)
5. Cite sources when applicable

Ensure questions test understanding, not just memorization.
Avoid trademarked content and proprietary question stems.
"""
```

### 3.4 Flashcard Agent

**Purpose**: Create testable flashcards with spaced repetition data.

**Capabilities**:
- Card type generation (basic, cloze, image occlusion)
- Deduplication against existing cards
- SRS parameter calculation (initial ease, interval)
- Testability verification

**Tools**:
- `card_dedupe`: Check for duplicate cards
- `cloze_generator`: Create cloze deletion cards
- `srs_calculator`: Calculate SRS parameters
- `testability_check`: Verify card is testable

**Instruction Pattern**:
```python
instruction = """
You are the Flashcard Agent. Create effective study cards.

Card types:
- Basic: Front/Back fact pairs
- Cloze: Fill-in-the-blank with context
- Image Occlusion: Identify labeled structures

For each card:
1. Focus on a single, testable fact
2. Make the front clear and specific
3. Provide context on the back (not just the answer)
4. Include source reference
5. Avoid ambiguous phrasing

Deduplicate against existing cards before creating new ones.
"""
```

### 3.5 Scheduler Agent

**Purpose**: Manage study schedules with adaptive rescheduling.

**Capabilities**:
- Task generation based on user goals and timeline
- Workload prediction and burnout prevention
- Overdue task rescheduling
- Calendar integration (ICS export, Google Calendar)

**Tools**:
- `task_generator`: Create study tasks
- `workload_predictor`: Estimate daily study time
- `reschedule_engine`: Adjust schedule for missed tasks
- `calendar_sync`: Export to calendar systems

**Instruction Pattern**:
```python
instruction = """
You are the Scheduler Agent. Create adaptive study plans.

Consider:
- User's exam timeline and goals
- Daily time availability and constraints
- Spaced repetition requirements
- Burnout prevention (avoid overload)
- Rescheduling flexibility for missed tasks

Generate realistic, achievable daily task lists.
Alert users when workload becomes excessive.
Support ICS export for external calendar integration.
"""
```

---

## 4. Background Agents

### 4.1 Document Parser Agent

**Purpose**: Process uploaded documents (PDF, DOCX, images) for ingestion.

**Capabilities**:
- OCR for scanned documents and images
- Text extraction with structure preservation
- Metadata extraction (title, authors, headings)
- Page and section boundary detection

**Tools**:
- `ocr_engine`: Text extraction from images
- `pdf_parser`: PDF structure extraction
- `metadata_extract`: Extract document metadata
- `chunker`: Split document into chunks

### 4.2 Embedding Agent

**Purpose**: Generate and manage vector embeddings for retrieval.

**Capabilities**:
- Embedding generation for document chunks
- Vector upsert to database
- Embedding quality verification
- Bulk re-embedding for model updates

**Tools**:
- `embedding_model`: Generate embeddings (Vertex AI)
- `vector_store`: Upsert to vector database
- `quality_check`: Verify embedding quality

### 4.3 Notification Agent

**Purpose**: Send review reminders and study notifications.

**Capabilities**:
- Review due reminders
- Task scheduled notifications
- Streak protection alerts
- Quiet hours and frequency cap enforcement

**Tools**:
- `email_sender`: Send email notifications
- `push_sender`: Send push notifications
- `user_prefs`: Respect user preferences

---

## 5. A2A Remote Agents

### 5.1 Analytics Agent (Remote)

**Purpose**: Progress tracking and learning analytics (computationally intensive).

**Why Remote**: Heavy ML computations and complex analytics benefit from dedicated resources.

**A2A Endpoint**: `https://analytics.medmentor.com/a2a`

**Capabilities**:
- Progress tracking across all activities
- Weakness identification and targeted recommendations
- Exam readiness prediction
- Learning efficiency metrics

**Tools**:
- `progress_db`: User progress data
- `ml_models`: Prediction models
- `reporting`: Analytics dashboards

### 5.2 Scheduler Agent (Remote - Optional)

**Purpose**: Heavy schedule optimization computations.

**Why Remote**: Complex scheduling algorithms can be resource-intensive.

**A2A Endpoint**: `https://scheduler.medmentor.com/a2a`

---

## 6. Agent Communication Patterns

### 6.1 Sequential Flow

```
User → Supervisor → Retrieval → Tutor → Safety → Response
```

### 6.2 Parallel Flow

```
User → Supervisor → [Quiz, Flashcard] → Safety → Aggregated Response
```

### 6.3 Hierarchical Flow

```
User → Supervisor
    → [Specialist Agents]
    → Safety (all paths converge)
    → Response
```

### 6.4 A2A Remote Flow

```
User → Supervisor → transfer_to_agent → Remote Agent → Response
```

---

## 7. Agent Testing

### 7.1 Unit Tests

Each agent should have:
- Input/output schema validation
- Tool invocation tests
- State management tests
- Error handling tests

### 7.2 Integration Tests

- Agent handoff tests
- Multi-agent workflow tests
- A2A communication tests
- End-to-end user request tests

### 7.3 Safety Tests

- Refusal correctness for prohibited requests
- Citation accuracy verification
- Hallucination detection
- Prompt injection resilience

### 7.4 Evaluation Framework

See `docs/AI_RULES.md` for evaluation requirements and thresholds.

---

## Appendix: Agent Configuration Examples

### Example: Supervisor Agent Configuration

```python
from google.adk.agents import LlmAgent
from google.adk.tools import Tool

supervisor_agent = LlmAgent(
    name="SupervisorAgent",
    instruction="""You are the MedMentor Supervisor Agent...""",
    tools=[
        Tool("classify_intent", classify_intent_function),
        Tool("aggregate_response", aggregate_function),
        Tool("transfer_to_agent", a2a_transfer_function)
    ],
    model="gemini-2.5-flash",
    output_key="orchestrated_response"
)
```

### Example: A2A Remote Agent Call

```python
# On orchestrator server
from google.adk.agents import RemoteAgent

analytics_agent = RemoteAgent(
    name="AnalyticsAgent",
    endpoint="https://analytics.medmentor.com/a2a",
    auth_token=env.A2A_AUTH_TOKEN
)

# Call via A2A protocol
result = await analytics_agent.invoke(
    input={
        "action": "get_progress_insights",
        "user_id": user_id
    }
)
```

---

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [A2A Protocol](/Users/v13478/Desktop/MedMentor/docs/A2A_PROTOCOL.md)
- [Agent Studio](/Users/v13478/Desktop/MedMentor/docs/AGENT_STUDIO.md)
- [Architecture](/Users/v13478/Desktop/MedMentor/docs/ARCHITECTURE.md)
