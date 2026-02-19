# MedMentor AI — A2A Protocol Specification

**Version:** 0.0.1
**Last Updated:** 2026-02-19

This document specifies the **Agent-to-Agent (A2A)** protocol implementation in MedMentor using Google ADK for inter-agent communication.

---

## 1. Overview

### 1.1 What is A2A?

**Agent-to-Agent (A2A)** is Google's open protocol for direct agent-to-agent communication and collaboration. It enables:

- **Scalability**: Run agents on separate servers
- **Modularity**: Develop and deploy agents independently
- **Flexibility**: Mix local and remote agents seamlessly
- **Security**: Authenticated agent-to-agent communication

### 1.2 A2A in MedMentor

MedMentor uses A2A for:
- Horizontal scaling of high-demand agents (Analytics, Scheduler)
- Independent deployment and updates
- Multi-language agent support (Python, TypeScript, Java, Go)
- Resource isolation for computationally intensive operations

---

## 2. Architecture

### 2.1 Local vs Remote Agents

```
┌─────────────────────────────────────────────────────────┐
│              MedMentor Agent Orchestrator               │
│                                                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │    Tutor    │  │    Quiz      │  │  Flashcard   │  │
│  │   Agent     │  │   Agent      │  │   Agent      │  │
│  │  (Local)    │  │  (Local)     │  │  (Local)     │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                           │
│  ┌────────────────────────────────────────────────────┐   │
│  │           A2A Protocol Layer                      │   │
│  │  • transfer_to_agent tool                         │   │
│  │  • Authentication & Authorization                  │   │
│  │  • Retry & Timeout Handling                        │   │
│  └────────────────────────────────────────────────────┘   │
│                          │                                │
│                          ▼                                │
│  ┌────────────────────────────────────────────────────┐   │
│  │         A2A Remote Agents (Separate Servers)      │   │
│  │                                                          │
│  │  ┌─────────────┐         ┌─────────────┐          │   │
│  │  │ Analytics   │         │ Scheduler   │          │   │
│  │  │   Agent     │         │   Agent     │          │   │
│  │  │  /a2a       │         │  /a2a       │          │   │
│  │  └─────────────┘         └─────────────┘          │   │
│  └────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 2.2 Communication Patterns

#### Pattern 1: Simple Remote Operation
```
Supervisor → transfer_to_agent("AnalyticsAgent") → Remote Analytics → Response
```

#### Pattern 2: Sequential Agent Operations
```
User → Supervisor → Retrieval (local) → transfer_to_agent("Analytics") → Analytics → Response
```

#### Pattern 3: Combined Local and Remote
```
User → Supervisor → [Quiz (local), Analytics (remote via A2A)] → Aggregated Response
```

---

## 3. Protocol Specification

### 3.1 A2A Endpoint

Remote agents expose an A2A endpoint:

```
POST https://{service}.medmentor.com/a2a
Content-Type: application/json
Authorization: Bearer {A2A_AUTH_TOKEN}
```

### 3.2 Request Format

```json
{
  "agent_name": "AnalyticsAgent",
  "agent_version": "1.0.0",
  "conversation_id": "conv_abc123",
  "input": {
    "action": "get_progress_insights",
    "user_id": "user_123",
    "parameters": {
      "timeframe": "7d",
      "include_recommendations": true
    }
  },
  "context": {
    "user_preferences": {...},
    "conversation_history": [...]
  }
}
```

### 3.3 Response Format

```json
{
  "agent_name": "AnalyticsAgent",
  "conversation_id": "conv_abc123",
  "output": {
    "insights": [...],
    "recommendations": [...],
    "metrics": {...}
  },
  "status": "success",
  "execution_time_ms": 245
}
```

### 3.4 Error Format

```json
{
  "agent_name": "AnalyticsAgent",
  "conversation_id": "conv_abc123",
  "error": {
    "code": "TIMEOUT",
    "message": "Agent execution exceeded timeout",
    "retryable": true
  },
  "status": "error"
}
```

---

## 4. Implementation

### 4.1 Local Agent: Calling Remote Agent

```python
from google.adk.agents import LlmAgent
from google.adk.tools import Tool

async def transfer_to_analytics(context: dict) -> dict:
    """Transfer execution to remote Analytics agent via A2A."""
    import httpx

    async with httpx.AsyncClient() as client:
        response = await client.post(
            "https://analytics.medmentor.com/a2a",
            json={
                "agent_name": "AnalyticsAgent",
                "conversation_id": context["conversation_id"],
                "input": {
                    "action": "get_progress_insights",
                    "user_id": context["user_id"]
                },
                "context": context
            },
            headers={"Authorization": f"Bearer {context['a2a_token']}"}
        )
        return response.json()

supervisor_agent = LlmAgent(
    name="SupervisorAgent",
    instruction="...",
    tools=[
        Tool("transfer_to_analytics", transfer_to_analytics)
    ]
)
```

### 4.2 Remote Agent: A2A Server Implementation

```python
from fastapi import FastAPI, Header, HTTPException
from pydantic import BaseModel

app = FastAPI()

class A2ARequest(BaseModel):
    agent_name: str
    conversation_id: str
    input: dict
    context: dict = {}

@app.post("/a2a")
async def a2a_endpoint(
    request: A2ARequest,
    authorization: str = Header(...)
):
    # Verify A2A token
    if not verify_a2a_token(authorization):
        raise HTTPException(status_code=401, detail="Invalid A2A token")

    # Route to appropriate agent
    agent = get_agent(request.agent_name)

    # Execute agent
    result = await agent.invoke(
        input=request.input,
        context=request.context
    )

    return {
        "agent_name": request.agent_name,
        "conversation_id": request.conversation_id,
        "output": result,
        "status": "success"
    }
```

### 4.3 ADK Remote Agent Client

```python
from google.adk.agents import RemoteAgent

# Create remote agent reference
analytics_agent = RemoteAgent(
    name="AnalyticsAgent",
    endpoint="https://analytics.medmentor.com/a2a",
    auth_token=env.A2A_AUTH_TOKEN
)

# Use in supervisor
supervisor_agent = LlmAgent(
    name="SupervisorAgent",
    instruction="...",
    sub_agents=[analytics_agent]  # Can use like local agents
)
```

---

## 5. Security

### 5.1 Authentication

- **Bearer Token**: A2A tokens for service-to-service authentication
- **Token Rotation**: Regular rotation of A2A tokens
- **Per-Agent Tokens**: Each remote agent has unique tokens
- **Token Scopes**: Tokens are scoped to specific actions

### 5.2 Authorization

- **Agent Whitelist**: Supervisor can only call whitelisted remote agents
- **Action Validation**: Remote agents validate requested actions
- **User Context**: User identity passed through for authorization

### 5.3 Encryption

- **TLS**: All A2A communication over HTTPS
- **Signed Payloads**: Optional request signing for verification

---

## 6. Reliability

### 6.1 Timeout Handling

- **Request Timeout**: 30 seconds default for remote agent calls
- **Retry Logic**: Exponential backoff for retryable errors
- **Fallback**: Graceful degradation when remote agents unavailable

### 6.2 Circuit Breaker

- **Failure Threshold**: Circuit opens after N consecutive failures
- **Recovery**: Circuit closes after health check succeeds
- **Fallback Behavior**: Use local fallback or return cached response

### 6.3 Observability

- **Tracing**: Distributed tracing across agent boundaries
- **Logging**: All A2A calls logged with request/response
- **Metrics**: Latency, success rate, error rate per remote agent

---

## 7. Deployment

### 7.1 Remote Agent Service

```yaml
# Kubernetes Deployment (example)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: analytics-agent
spec:
  replicas: 3
  selector:
    matchLabels:
      app: analytics-agent
  template:
    metadata:
      labels:
        app: analytics-agent
    spec:
      containers:
      - name: analytics-agent
        image: medmentor/analytics-agent:latest
        ports:
        - containerPort: 8080
        env:
        - name: A2A_TOKEN
          valueFrom:
            secretKeyRef:
              name: a2a-secrets
              key: analytics-token
---
apiVersion: v1
kind: Service
metadata:
  name: analytics-agent
spec:
  selector:
    app: analytics-agent
  ports:
  - port: 443
    targetPort: 8080
  type: LoadBalancer
```

### 7.2 Load Balancing

- **Round-robin**: Distribute load across multiple instances
- **Sticky Sessions**: Route requests from same conversation to same instance
- **Health Checks**: Regular health checks to remove unhealthy instances

---

## 8. Testing

### 8.1 Unit Tests

```python
async def test_a2a_transfer():
    result = await transfer_to_analytics({
        "conversation_id": "test",
        "user_id": "user123",
        "a2a_token": "test_token"
    })
    assert result["status"] == "success"
```

### 8.2 Integration Tests

```python
async def test_supervisor_to_remote_agent():
    supervisor = SupervisorAgent()
    response = await supervisor.invoke({
        "message": "What's my progress?",
        "user_id": "user123"
    })
    assert "analytics" in response["source_agents"]
```

### 8.3 Load Tests

- Concurrent request handling
- Timeout under load
- Circuit breaker behavior

---

## 9. Best Practices

### 9.1 When to Use A2A

Use remote agents via A2A when:
- Agent requires significant computational resources
- Agent needs to be scaled independently
- Agent is implemented in a different language
- Agent needs dedicated infrastructure (GPUs, special databases)

### 9.2 When NOT to Use A2A

Avoid A2A for:
- Simple, fast operations (network overhead not justified)
- Low-latency requirements (< 100ms)
- Tightly coupled operations that need shared memory

### 9.3 Performance Optimization

- **Batch Operations**: Combine multiple requests into single A2A call
- **Caching**: Cache remote agent responses when appropriate
- **Connection Pooling**: Reuse HTTP connections

---

## 10. Troubleshooting

### 10.1 Common Issues

| Issue | Cause | Solution |
|-------|--------|----------|
| Timeout | Remote agent slow | Increase timeout, optimize agent |
| Authentication Error | Invalid token | Verify token, check rotation |
| Not Found | Endpoint misconfigured | Verify A2A endpoint URL |
| Rate Limited | Too many requests | Implement rate limiting, caching |

### 10.2 Debugging

- **Logs**: Check both local and remote agent logs
- **Tracing**: Use distributed tracing IDs
- **Health Checks**: Verify remote agent health status

---

## References

- [Google ADK A2A Documentation](https://google.github.io/adk-docs/a2a)
- [Agent Catalog](/Users/v13478/Desktop/MedMentor/docs/AGENTS.md)
- [Architecture](/Users/v13478/Desktop/MedMentor/docs/ARCHITECTURE.md)
