# 🔍 Day 4: Agent Observability

<div align="center">

![Day 4](https://img.shields.io/badge/Day-4-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-red?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-2.5%20hours-orange?style=for-the-badge)

**Monitor, Debug, and Optimize Your AI Agents**

[← Day 3](../Day-3) | [Back to Main](../README.md) | [Day 5 →](../Day-5)

</div>

---

## 🎯 Learning Objectives

By the end of this day, you will:

- ✅ Implement comprehensive logging for agent activities
- ✅ Monitor agent performance and resource usage
- ✅ Debug complex agent behaviors and errors
- ✅ Trace multi-step agent reasoning
- ✅ Set up alerts and metrics collection
- ✅ Optimize agent costs and latency
- ✅ Build production monitoring dashboards

---

## 📓 Notebook

**[`day-4a-agent-observability.ipynb`](./day-4a-agent-observability.ipynb)**

This notebook covers monitoring, debugging, and optimizing AI agents in production environments.

---

## 📚 Topics Covered

### 1. Introduction to Observability
- **What is Agent Observability?**
  - Logging vs monitoring vs observability
  - The three pillars: logs, metrics, traces
  - Why observability matters for agents
  - Observability in production systems

### 2. Logging Best Practices
- **Structured Logging**
  - Log levels and when to use them
  - Contextual information capture
  - Sensitive data handling
  - Log aggregation strategies

### 3. Performance Monitoring
- **Metrics Collection**
  - Response time tracking
  - Token usage monitoring
  - Cost per request calculation
  - Success/failure rates
  - Tool execution metrics

### 4. Distributed Tracing
- **Request Flow Tracking**
  - Trace IDs and span management
  - Parent-child relationships
  - Cross-service tracing
  - Visualizing agent workflows

### 5. Debugging Techniques
- **Troubleshooting Agents**
  - Common failure patterns
  - Step-by-step execution debugging
  - Prompt and response analysis
  - Tool call inspection

### 6. Alerting and Incident Response
- **Proactive Monitoring**
  - Setting up alerts
  - Threshold configuration
  - On-call procedures
  - Incident post-mortems

---

## 🛠️ Key Concepts

### The Three Pillars of Observability

```python
# 1. LOGS - What happened
logger.info("Agent started processing", extra={
    "session_id": session.id,
    "user_query": query,
    "timestamp": datetime.now()
})

# 2. METRICS - How much/how fast
metrics.record_latency("agent.response_time", duration_ms)
metrics.increment("agent.requests.total")

# 3. TRACES - Where time is spent
with tracer.start_span("agent.process_query") as span:
    span.set_attribute("query.length", len(query))
    result = await agent.process(query)
```

### Structured Logging

```python
import logging
import json
from datetime import datetime

class StructuredLogger:
    def __init__(self, name):
        self.logger = logging.getLogger(name)
        
    def log_agent_action(self, action, **kwargs):
        """Log agent actions with structured data."""
        log_entry = {
            "timestamp": datetime.utcnow().isoformat(),
            "action": action,
            "level": "INFO",
            **kwargs
        }
        self.logger.info(json.dumps(log_entry))

# Usage
logger = StructuredLogger("agent")
logger.log_agent_action(
    "tool_call",
    tool_name="get_weather",
    parameters={"location": "San Francisco"},
    session_id="user_123"
)
```

### Performance Metrics

| Metric | Description | Target | Alert Threshold |
|--------|-------------|--------|-----------------|
| **Response Time** | End-to-end latency | < 2s | > 5s |
| **Token Usage** | Tokens per request | < 1000 | > 5000 |
| **Cost per Request** | $ per query | < $0.01 | > $0.05 |
| **Error Rate** | Failed requests % | < 1% | > 5% |
| **Tool Success Rate** | Successful tool calls | > 95% | < 90% |

---

## 🏗️ Observability Architecture

### Architecture 1: Basic Logging

```
Agent → Console Logger → Log File
```

**Best for**: Development, simple applications.

### Architecture 2: Centralized Logging

```
Agent → Logger → Log Aggregator → Dashboard
                     (ELK/Splunk)
```

**Best for**: Production, multiple agents.

### Architecture 3: Full Observability Stack

```
Agent → OpenTelemetry → Collector → Multiple Backends
                                    ├─ Logs (Loki)
                                    ├─ Metrics (Prometheus)
                                    └─ Traces (Jaeger)
```

**Best for**: Enterprise, distributed systems.

---

## 💡 Practical Examples

### Example 1: Request Tracing System

Build comprehensive tracing for agent requests.

**Components:**
- Request ID generation
- Span creation for each step
- Timing measurement
- Error tracking

**Implementation:**
```python
import uuid
from contextlib import contextmanager
from datetime import datetime

class AgentTracer:
    def __init__(self):
        self.traces = {}
    
    @contextmanager
    def trace_request(self, operation):
        """Trace an agent operation."""
        trace_id = str(uuid.uuid4())
        start_time = datetime.now()
        
        try:
            self.traces[trace_id] = {
                "operation": operation,
                "start_time": start_time,
                "spans": []
            }
            yield trace_id
        finally:
            duration = (datetime.now() - start_time).total_seconds()
            self.traces[trace_id]["duration"] = duration
            self.log_trace(trace_id)
    
    def add_span(self, trace_id, name, data=None):
        """Add a span to a trace."""
        span = {
            "name": name,
            "timestamp": datetime.now(),
            "data": data or {}
        }
        self.traces[trace_id]["spans"].append(span)

# Usage
tracer = AgentTracer()

async def process_with_tracing(query):
    with tracer.trace_request("agent.query") as trace_id:
        tracer.add_span(trace_id, "parse_query", {"query": query})
        
        # Process query
        result = await agent.process(query)
        
        tracer.add_span(trace_id, "generate_response", {
            "tokens": result.token_count
        })
        
        return result
```

### Example 2: Cost Monitoring Dashboard

Track and visualize agent costs in real-time.

**Features:**
- Token usage per session
- Cost calculation
- Budget alerts
- Usage patterns

**Metrics Collection:**
```python
class CostMonitor:
    def __init__(self):
        self.costs = []
        self.token_prices = {
            "input": 0.00001,  # per token
            "output": 0.00003
        }
    
    def record_usage(self, session_id, input_tokens, output_tokens):
        """Record token usage and calculate cost."""
        cost = (
            input_tokens * self.token_prices["input"] +
            output_tokens * self.token_prices["output"]
        )
        
        entry = {
            "session_id": session_id,
            "timestamp": datetime.now(),
            "input_tokens": input_tokens,
            "output_tokens": output_tokens,
            "cost": cost
        }
        
        self.costs.append(entry)
        
        # Check budget alert
        if cost > 0.10:
            self.send_alert(f"High cost query: ${cost:.4f}")
        
        return cost
    
    def get_session_cost(self, session_id):
        """Get total cost for a session."""
        return sum(
            entry["cost"] 
            for entry in self.costs 
            if entry["session_id"] == session_id
        )
```

### Example 3: Error Tracking and Analysis

Comprehensive error monitoring system.

**Features:**
- Error categorization
- Stack trace capture
- Error frequency tracking
- Auto-retry logic

**Implementation:**
```python
from enum import Enum
from collections import defaultdict

class ErrorCategory(Enum):
    API_ERROR = "api_error"
    TOOL_ERROR = "tool_error"
    VALIDATION_ERROR = "validation_error"
    TIMEOUT = "timeout"
    RATE_LIMIT = "rate_limit"

class ErrorTracker:
    def __init__(self):
        self.errors = []
        self.error_counts = defaultdict(int)
    
    def record_error(self, category, error, context=None):
        """Record an error with context."""
        error_entry = {
            "timestamp": datetime.now(),
            "category": category.value,
            "message": str(error),
            "context": context or {}
        }
        
        self.errors.append(error_entry)
        self.error_counts[category.value] += 1
        
        # Alert on high error rate
        if self.error_counts[category.value] > 10:
            self.send_alert(
                f"High error rate for {category.value}: "
                f"{self.error_counts[category.value]} errors"
            )
    
    def get_error_summary(self):
        """Get summary of errors."""
        return {
            "total_errors": len(self.errors),
            "by_category": dict(self.error_counts),
            "recent_errors": self.errors[-5:]
        }

# Usage with retry logic
async def call_with_monitoring(agent, query, tracker):
    max_retries = 3
    
    for attempt in range(max_retries):
        try:
            return await agent.process(query)
        except TimeoutError as e:
            tracker.record_error(
                ErrorCategory.TIMEOUT,
                e,
                {"attempt": attempt, "query": query}
            )
            if attempt < max_retries - 1:
                await asyncio.sleep(2 ** attempt)  # Exponential backoff
        except Exception as e:
            tracker.record_error(
                ErrorCategory.API_ERROR,
                e,
                {"query": query}
            )
            raise
```

---

## 🚀 Getting Started

### Prerequisites

```bash
# Install observability packages
pip install opentelemetry-api opentelemetry-sdk
pip install prometheus-client
pip install structlog
```

### Basic Setup

```python
import structlog
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import ConsoleSpanExporter, BatchSpanProcessor

# Setup structured logging
structlog.configure(
    processors=[
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.JSONRenderer()
    ]
)
logger = structlog.get_logger()

# Setup tracing
trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

# Add span exporter
span_processor = BatchSpanProcessor(ConsoleSpanExporter())
trace.get_tracer_provider().add_span_processor(span_processor)
```

---

## 🎓 Exercises

### Exercise 1: Build a Logging Framework

Create a comprehensive logging system for agents:
- Structured JSON logging
- Different log levels
- Context propagation
- Log rotation

**Challenge**: Add PII (Personally Identifiable Information) filtering.

### Exercise 2: Performance Dashboard

Build a dashboard that shows:
- Real-time request rate
- Average response time
- Token usage trends
- Error rates

**Challenge**: Add cost projections based on trends.

### Exercise 3: Distributed Trace Visualizer

Implement tracing that shows:
- Request flow through components
- Time spent in each step
- Tool call dependencies
- Error locations

**Challenge**: Create a visual timeline view.

---

## 📊 Monitoring Dashboards

### Key Metrics to Track

**Operational Metrics:**
```python
# Request volume
total_requests = Counter('agent_requests_total')
active_requests = Gauge('agent_requests_active')

# Latency
response_time = Histogram('agent_response_seconds')
tool_call_time = Histogram('agent_tool_call_seconds')

# Errors
error_count = Counter('agent_errors_total', ['category'])
```

**Business Metrics:**
```python
# Usage patterns
sessions_per_hour = Gauge('agent_sessions_per_hour')
queries_per_session = Histogram('agent_queries_per_session')

# Cost metrics
cost_per_session = Histogram('agent_cost_per_session')
total_spend = Counter('agent_total_cost_dollars')
```

### Dashboard Layout

```
┌─────────────────────────────────────────┐
│           Agent Overview                │
├─────────────────────────────────────────┤
│ Requests/min  │ Avg Latency │ Error %  │
│     125       │    1.2s     │   0.5%   │
├─────────────────────────────────────────┤
│        Response Time (24h)              │
│  [========== Graph ==========]          │
├─────────────────────────────────────────┤
│  Top Errors    │   Token Usage          │
│  • Timeout: 5  │   • Input: 125K        │
│  • API: 2      │   • Output: 89K        │
└─────────────────────────────────────────┘
```

---

## 🔍 Debugging Workflows

### Step 1: Reproduce the Issue

```python
def capture_debug_context(agent_call):
    """Capture full context for debugging."""
    return {
        "input": agent_call.query,
        "session_state": agent_call.session.to_dict(),
        "tools_available": agent_call.agent.tools,
        "model_config": agent_call.agent.config
    }
```

### Step 2: Trace Execution

```python
with tracer.start_as_current_span("debug_agent_call"):
    # Enable verbose logging
    logger.setLevel(logging.DEBUG)
    
    # Run with full trace
    result = await agent.process(query)
    
    # Examine traces
    print_trace_tree(tracer.get_current_trace())
```

### Step 3: Analyze Logs

```python
def analyze_failed_request(trace_id):
    """Analyze a failed request."""
    trace = get_trace(trace_id)
    
    print(f"Request failed at: {trace.failure_point}")
    print(f"Error: {trace.error}")
    print(f"Tool calls made: {trace.tool_calls}")
    print(f"Last successful step: {trace.last_success}")
```

---

## 🔔 Alerting Strategies

### Alert Configuration

```python
class AlertConfig:
    rules = [
        {
            "name": "High Error Rate",
            "condition": "error_rate > 5%",
            "window": "5m",
            "severity": "critical"
        },
        {
            "name": "Slow Response",
            "condition": "p95_latency > 5s",
            "window": "10m",
            "severity": "warning"
        },
        {
            "name": "High Cost",
            "condition": "hourly_cost > $10",
            "window": "1h",
            "severity": "warning"
        }
    ]
```

### Alert Handlers

```python
async def handle_alert(alert):
    """Handle different alert types."""
    if alert.severity == "critical":
        # Page on-call engineer
        await send_page(alert)
        
        # Auto-scale if needed
        if "high_load" in alert.tags:
            await scale_up_agents()
    
    elif alert.severity == "warning":
        # Send to Slack
        await send_slack_notification(alert)
        
    # Always log
    logger.warning("Alert triggered", alert=alert.to_dict())
```

---

## 📖 Key Takeaways

- 🔑 **Observability is essential** for production agents
- 🔑 **Structured logging** enables better analysis
- 🔑 **Distributed tracing** reveals complex workflows
- 🔑 **Metrics and alerts** enable proactive monitoring
- 🔑 **Cost tracking** prevents budget overruns
- 🔑 **Debug workflows** speed up issue resolution

---

## 🔗 Additional Resources

### Documentation
- [OpenTelemetry Python](https://opentelemetry.io/docs/instrumentation/python/)
- [Prometheus Best Practices](https://prometheus.io/docs/practices/)
- [Structured Logging with structlog](https://www.structlog.org/)

### Tools
- [Jaeger](https://www.jaegertracing.io/) - Distributed tracing
- [Grafana](https://grafana.com/) - Visualization dashboards
- [Sentry](https://sentry.io/) - Error tracking

### Articles
- [Observability for AI Systems](https://www.oreilly.com/library/view/observability-engineering/9781492076445/)
- [Monitoring LLM Applications](https://www.deeplearning.ai/short-courses/monitoring-llm-applications/)

---

## ❓ Common Issues & Solutions

<details>
<summary><b>Too Much Log Data</b></summary>

**Problem**: Logs growing too large

**Solution**:
- Implement log sampling
- Use appropriate log levels
- Set up log rotation
- Archive old logs
- Filter sensitive data

</details>

<details>
<summary><b>Missing Context in Traces</b></summary>

**Problem**: Can't understand trace flow

**Solution**:
- Add more span attributes
- Include query context
- Tag with session IDs
- Add custom annotations
- Use span events

</details>

<details>
<summary><b>Alert Fatigue</b></summary>

**Problem**: Too many false positive alerts

**Solution**:
- Tune alert thresholds
- Add alert grouping
- Implement smart routing
- Use severity levels
- Add runbook links

</details>

<details>
<summary><b>High Monitoring Cost</b></summary>

**Problem**: Observability infrastructure is expensive

**Solution**:
- Sample high-volume metrics
- Use metric aggregation
- Archive old data
- Optimize retention policies
- Use open-source tools

</details>

---

## 🎯 Next Steps

Ready for the final day? Continue to:

### [Day 5: Agent-to-Agent Communication →](../Day-5)

Learn about:
- Multi-agent architectures
- Inter-agent communication
- Task delegation patterns
- Agent orchestration
- Collaborative workflows

---

<div align="center">

[← Day 3](../Day-3) | [Back to Main](../README.md) | [Day 5 →](../Day-5)

**Keep Monitoring!** 📊

</div>
