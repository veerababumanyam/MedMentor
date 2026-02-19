# MedMentor AI — Google ADK Setup Guide

**Version:** 0.0.1
**Last Updated:** 2026-02-19

This document provides step-by-step instructions for setting up **Google Agent Development Kit (ADK)** for MedMentor development.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Python Setup](#2-python-setup)
3. [TypeScript Setup](#3-typescript-setup)
4. [Vertex AI Configuration](#4-vertex-ai-configuration)
5. [First Agent](#5-first-agent)
6. [Local Development](#6-local-development)
7. [Deployment](#7-deployment)

---

## 1. Prerequisites

### 1.1 Requirements

- **Python**: 3.11+ or **Node.js**: 20+
- **Google Cloud Project**: With Vertex AI API enabled
- **Authentication**: gcloud CLI configured
- **Database**: PostgreSQL 16+ with pgvector
- **Redis**: For caching and queues

### 1.2 Google Cloud Setup

```bash
# Install gcloud CLI
curl https://sdk.cloud.google.com | bash

# Initialize
gcloud init

# Authenticate
gcloud auth login
gcloud auth application-default login

# Enable Vertex AI API
gcloud services enable aiplatform.googleapis.com
```

---

## 2. Python Setup

### 2.1 Installation

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install ADK
pip install google-adk

# Install dependencies
pip install fastapi uvicorn psycopg2-binary redis python-jose[cryptography]
```

### 2.2 Environment Variables

```bash
# .env file
GOOGLE_PROJECT_ID=your-project-id
GOOGLE_LOCATION=us-central1
VERTEX_AI_ENDPOINT=us-central1-aiplatform.googleapis.com

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/medmentor
REDIS_URL=redis://localhost:6379/0

# A2A (if using remote agents)
A2A_AUTH_TOKEN=your-a2a-token-here
```

### 2.3 Basic Project Structure

```
medmentor/
├── agents/
│   ├── __init__.py
│   ├── supervisor.py
│   ├── tutor.py
│   ├── retrieval.py
│   ├── safety.py
│   └── tools/
│       ├── __init__.py
│       ├── vector_search.py
│       └── citations.py
├── api/
│   └── main.py
├── models/
│   └── schema.py
└── config.py
```

---

## 3. TypeScript Setup

### 3.1 Installation

```bash
# Initialize project
npm init -y
npm install @google-cloud/vertexai
npm install @google-cloud/aiplatform
npm install express typescript @types/node
```

### 3.2 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### 3.3 Environment Setup

```bash
# .env
GOOGLE_APPLICATION_CREDENTIALS=./path/to/service-account.json
PROJECT_ID=your-project-id
```

---

## 4. Vertex AI Configuration

### 4.1 Model Configuration

```python
# config.py
from google.cloud import aiplatform

# Vertex AI configuration
PROJECT_ID = "your-project-id"
LOCATION = "us-central1"
MODEL_NAME = "gemini-2.5-flash"  # or "gemini-2.5-pro"

# Model router with fallback
MODEL_ROUTER = {
    "primary": "projects/{project}/locations/{location}/publishers/google/models/{model}",
    "fallbacks": [
        "openai/gpt-4o",
        "anthropic/claude-3-5-sonnet"
    ]
}
```

### 4.2 LiteLLM Setup (Multi-Provider)

```bash
# Install LiteLLM for model routing
pip install litellm

# Configure model router
cat > litellm_config.yaml << EOF
model_list:
  - model_name: gemini-2.5-flash
    litellm_params:
      model: vertex_ai/gemini-2.5-flash
      vertex_project: $PROJECT_ID
      vertex_location: $LOCATION
  - model_name: gpt-4o
    litellm_params:
      model: openai/gpt-4o
  - model_name: claude-3-5-sonnet
    litellm_params:
      model: anthropic/claude-3-5-sonnet
EOF
```

---

## 5. First Agent

### 5.1 Basic Tutor Agent

```python
# agents/tutor.py
from google.adk.agents import LlmAgent
from google.adk.tools import Tool

def get_tutor_agent():
    """Create and return the Tutor agent."""

    return LlmAgent(
        name="TutorAgent",
        instruction="""
        You are a medical education Tutor using the Socratic method.

        Provide explanations at three depth levels:
        - Simple: Layperson-friendly
        - Student: Medical learner
        - Clinical: Professional

        Always cite sources and admit uncertainty.
        """,
        tools=[
            Tool("search_knowledge", search_knowledge_function),
            Tool("get_citations", get_citations_function)
        ],
        model="gemini-2.5-flash",
        output_key="explanation"
    )
```

### 5.2 Supervisor Agent

```python
# agents/supervisor.py
from google.adk.agents import LlmAgent, SequentialAgent
from google.adk.agents import RemoteAgent

def get_supervisor_agent():
    """Create and return the Supervisor agent."""

    # Import local agents
    from agents.tutor import get_tutor_agent
    from agents.retrieval import get_retrieval_agent
    from agents.safety import get_safety_agent

    # Import remote agent (A2A)
    analytics_agent = RemoteAgent(
        name="AnalyticsAgent",
        endpoint="https://analytics.medmentor.com/a2a",
        auth_token=os.getenv("A2A_AUTH_TOKEN")
    )

    return SequentialAgent(
        name="SupervisorAgent",
        instruction="""
        You are the MedMentor Supervisor Agent.
        Route user requests to appropriate specialist agents.
        Ensure all outputs pass through the Safety Agent.
        """,
        sub_agents=[
            get_tutor_agent(),
            get_retrieval_agent(),
            analytics_agent
        ]
    )
```

---

## 6. Local Development

### 6.1 Run ADK Agent Locally

```python
# run_agent.py
import asyncio
from agents.supervisor import get_supervisor_agent

async def main():
    supervisor = get_supervisor_agent()

    response = await supervisor.invoke({
        "input": "Explain myocardial infarction",
        "user_id": "user_123",
        "conversation_id": "conv_abc"
    })

    print(response)

if __name__ == "__main__":
    asyncio.run(main())
```

### 6.2 Development Server

```bash
# Run agent server
python -m agents.server

# Or with uvicorn
uvicorn api.main:app --reload
```

### 6.3 Testing

```python
# tests/test_agents.py
import pytest
from agents.tutor import get_tutor_agent

@pytest.mark.asyncio
async def test_tutor_agent():
    agent = get_tutor_agent()

    response = await agent.invoke({
        "input": "What is myocardial infarction?",
        "user_id": "test_user"
    })

    assert "myocardial" in response["explanation"]
    assert len(response.get("citations", [])) > 0
```

---

## 7. Deployment

### 7.1 Docker Configuration

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

# Expose port
EXPOSE 8080

# Run application
CMD ["uvicorn", "api.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

### 7.2 Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
      - GOOGLE_APPLICATION_CREDENTIALS=/app/keys/service-account.json
    volumes:
      - ./keys:/app/keys:ro
    depends_on:
      - db
      - redis

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: medmentor
      POSTGRES_USER: medmentor
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

### 7.3 Cloud Run Deployment

```bash
# Build and deploy
gcloud builds submit --tag gcr.io/PROJECT_ID/medmentor-api

# Deploy to Cloud Run
gcloud run deploy medmentor-api \
  --image gcr.io/PROJECT_ID/medmentor-api \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars DATABASE_URL=$DATABASE_URL
```

---

## 8. Troubleshooting

### 8.1 Common Issues

| Issue | Solution |
|-------|----------|
| Authentication error | Run `gcloud auth application-default login` |
| Module not found | Ensure virtual environment is activated |
| Timeout error | Increase timeout in agent configuration |
| A2A connection error | Verify A2A token and endpoint URL |

### 8.2 Debug Mode

```python
# Enable debug logging
import logging
logging.basicConfig(level=logging.DEBUG)

# Run agent with debug output
agent = get_tutor_agent()
agent.debug = True
```

---

## 9. Resources

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Vertex AI Documentation](https://cloud.google.com/vertex-ai)
- [A2A Protocol](/Users/v13478/Desktop/MedMentor/docs/A2A_PROTOCOL.md)
- [Agent Catalog](/Users/v13478/Desktop/MedMentor/docs/AGENTS.md)
