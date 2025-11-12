# 🔨 Day 2: Agent Tools & Best Practices

<div align="center">

![Day 2](https://img.shields.io/badge/Day-2-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-3%20hours-orange?style=for-the-badge)

**Master Advanced Agent Patterns and Production Techniques**

[← Day 1](../Day-1) | [Back to Main](../README.md) | [Day 3 →](../Day-3)

</div>

---

## 🎯 Learning Objectives

By the end of this day, you will:

- ✅ Understand and implement Model Context Protocol (MCP)
- ✅ Build custom MCP servers and tools
- ✅ Handle long-running operations with async patterns
- ✅ Implement human-in-the-loop workflows
- ✅ Apply production-ready error handling
- ✅ Orchestrate multiple agents

---

## 📓 Notebook

**[`day-2b-agent-tools-best-practices.ipynb`](./day-2b-agent-tools-best-practices.ipynb)**

This notebook dives deep into advanced agent patterns and production best practices.

---

## 📚 Topics Covered

### 1. Model Context Protocol (MCP)
- **Introduction to MCP**
  - What is MCP and why it matters
  - MCP architecture and components
  - Benefits over custom tool implementations

### 2. Building MCP Servers
- **Custom Server Development**
  - Server setup and configuration
  - Tool registration and discovery
  - Resource management
  - Server lifecycle

### 3. Long-Running Operations
- **Async Patterns**
  - Understanding async/await
  - Background task management
  - Progress tracking
  - Cancellation handling

### 4. Human-in-the-Loop
- **Interactive Workflows**
  - Requesting user input
  - Confirmation patterns
  - Approval workflows
  - Timeout handling

### 5. Production Best Practices
- **Enterprise-Ready Agents**
  - Error handling strategies
  - Retry mechanisms
  - Logging and monitoring
  - Cost optimization
  - Security considerations

### 6. Agent Orchestration
- **Multi-Agent Systems**
  - Agent composition
  - Task delegation
  - Result aggregation
  - Conflict resolution

---

## 🛠️ Key Concepts

### Model Context Protocol (MCP)

MCP standardizes how agents interact with external tools:

```python
# Example: MCP Server Definition
class WeatherMCPServer:
    def __init__(self):
        self.tools = [
            "get_weather",
            "get_forecast",
            "get_alerts"
        ]
    
    async def handle_tool_call(self, tool_name, params):
        # Tool implementation
        pass
```

### Async Operations

Handle long-running tasks without blocking:

```python
async def long_running_task(query):
    """Execute a task that takes time."""
    result = await process_data(query)
    return result
```

### Human-in-the-Loop Pattern

```python
def agent_with_confirmation(action):
    """Agent that asks for confirmation."""
    print(f"About to perform: {action}")
    user_approval = input("Proceed? (y/n): ")
    
    if user_approval.lower() == 'y':
        return execute_action(action)
    else:
        return "Action cancelled by user"
```

---

## 🏗️ Architecture Patterns

### Pattern 1: Single Agent with MCP Server

```
User → Agent → MCP Server → External APIs
```

Simple architecture for focused tasks.

### Pattern 2: Multi-Agent Orchestration

```
User → Orchestrator Agent → Specialist Agents
                           → Each with own tools
```

Complex workflows with specialized agents.

### Pattern 3: Human-in-the-Loop

```
User → Agent → Tool Call → Confirmation Request
     ↑                              ↓
     └──────── User Approval ───────┘
```

Critical operations requiring approval.

---

## 💡 Practical Examples

### Example 1: Research Agent with MCP

Build an agent that can search, summarize, and compile research.

**MCP Tools:**
- `search_web(query)` - Web search
- `fetch_content(url)` - Get webpage content
- `summarize_text(text)` - Text summarization

**Workflow:**
1. User provides research topic
2. Agent searches for relevant sources
3. Fetches and summarizes content
4. Compiles final report

### Example 2: Data Analysis Pipeline

Create an agent that processes and analyzes data.

**MCP Tools:**
- `load_data(source)` - Load dataset
- `analyze_data(data, method)` - Run analysis
- `generate_visualization(data, type)` - Create charts

**Features:**
- Async data loading
- Progress tracking
- Result caching

### Example 3: Approval Workflow Agent

Build an agent for tasks requiring human approval.

**MCP Tools:**
- `create_document(template)` - Generate document
- `request_approval(document)` - Human approval
- `publish_document(document)` - Publish approved doc

**Human-in-the-Loop:**
- Document preview
- Approval/rejection
- Revision requests

---

## 🚀 Getting Started

### Prerequisites

```bash
# Install required packages
pip install google-adk mcp jupyter aiohttp
```

### MCP Server Setup

```python
from mcp import Server, Tool

# Create MCP server
server = Server(name="my-tools")

# Register tools
@server.tool()
async def my_tool(param: str) -> str:
    """Tool description."""
    return f"Result: {param}"

# Start server
await server.run()
```

---

## 🎓 Exercises

### Exercise 1: Build a File Processing MCP Server

Create an MCP server with tools for file operations:
- `read_file(path)` - Read file contents
- `write_file(path, content)` - Write to file
- `list_files(directory)` - List directory contents

**Challenge**: Add async file operations.

### Exercise 2: Create a Research Assistant

Build an agent that:
1. Takes a research question
2. Searches multiple sources
3. Compiles and summarizes findings
4. Asks for user feedback

**Challenge**: Implement progress updates.

### Exercise 3: Approval Workflow System

Implement an agent that:
1. Generates a report
2. Requests human approval
3. Handles revisions
4. Publishes final version

**Challenge**: Add timeout handling for approvals.

---

## 📊 Performance Optimization

### Caching Strategy

```python
from functools import lru_cache

@lru_cache(maxsize=100)
def expensive_operation(query):
    """Cache expensive results."""
    return compute_result(query)
```

### Parallel Execution

```python
import asyncio

async def parallel_tools():
    """Run multiple tools concurrently."""
    results = await asyncio.gather(
        tool1(),
        tool2(),
        tool3()
    )
    return results
```

### Rate Limiting

```python
from asyncio import Semaphore

semaphore = Semaphore(5)  # Max 5 concurrent calls

async def rate_limited_call():
    async with semaphore:
        return await api_call()
```

---

## 🔒 Security Best Practices

### Input Validation

```python
def validate_input(user_input: str) -> bool:
    """Validate and sanitize user input."""
    # Check for malicious patterns
    # Sanitize special characters
    # Validate format
    return True
```

### API Key Management

```python
import os
from dotenv import load_dotenv

load_dotenv()
API_KEY = os.getenv("API_KEY")
# Never hardcode keys!
```

### Rate Limiting & Quotas

- Implement per-user rate limits
- Track API usage
- Set spending caps
- Monitor for abuse

---

## 📖 Key Takeaways

- 🔑 **MCP standardizes** tool integration across agents
- 🔑 **Async patterns** enable responsive long-running operations
- 🔑 **Human-in-the-loop** adds control to critical workflows
- 🔑 **Error handling** is crucial for production reliability
- 🔑 **Monitoring and logging** enable debugging and optimization
- 🔑 **Security** must be built in from the start

---

## 🔗 Additional Resources

### Documentation
- [MCP Specification](https://modelcontextprotocol.io/docs)
- [ADK Advanced Patterns](https://google.github.io/adk-docs/patterns)
- [Async Python Guide](https://docs.python.org/3/library/asyncio.html)

### Tools & Libraries
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [ADK Examples](https://github.com/google/adk-examples)

### Case Studies
- [Building Production Agents](https://google.github.io/adk-docs/case-studies)
- [Enterprise MCP Deployments](https://modelcontextprotocol.io/case-studies)

---

## ❓ Common Issues & Solutions

<details>
<summary><b>MCP Server Not Connecting</b></summary>

**Problem**: Agent can't connect to MCP server

**Solution**:
- Verify server is running
- Check port configuration
- Ensure proper authentication
- Review firewall settings

</details>

<details>
<summary><b>Long Operations Timing Out</b></summary>

**Problem**: Operations failing due to timeout

**Solution**:
- Increase timeout limits
- Implement async patterns
- Add progress callbacks
- Use background tasks

</details>

<details>
<summary><b>Race Conditions in Async Code</b></summary>

**Problem**: Unexpected behavior with concurrent operations

**Solution**:
- Use proper async/await
- Implement locks/semaphores
- Avoid shared mutable state
- Test concurrent scenarios

</details>

---

## 🎯 Next Steps

Ready for the final day? Continue to:

### [Day 3: Agent Sessions & Memory →](../Day-3)

Learn about:
- Session management
- Conversation memory
- State persistence
- Context engineering

---

<div align="center">

[← Day 1](../Day-1) | [Back to Main](../README.md) | [Day 3 →](../Day-3)

**Keep Building!** 🚀

</div>
