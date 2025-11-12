# 💾 Day 3: Agent Sessions & Memory

<div align="center">

![Day 3](https://img.shields.io/badge/Day-3-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-red?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-2.5%20hours-orange?style=for-the-badge)

**Build Stateful Agents with Persistent Memory**

[← Day 2](../Day-2) | [Back to Main](../README.md)

</div>

---

## 🎯 Learning Objectives

By the end of this day, you will:

- ✅ Implement session management for agents
- ✅ Build agents with conversational memory
- ✅ Handle multi-turn conversations effectively
- ✅ Persist and restore agent state
- ✅ Optimize memory usage and performance
- ✅ Share state between multiple agents
- ✅ Deploy production-ready stateful agents

---

## 📓 Notebook

**[`day-3a-agent-sessions.ipynb`](./day-3a-agent-sessions.ipynb)**

This notebook covers advanced state management and memory systems for AI agents.

---

## 📚 Topics Covered

### 1. Session Management
- **Understanding Sessions**
  - What are agent sessions?
  - Session lifecycle (create, use, close)
  - Session vs stateless interactions
  - When to use sessions

### 2. Conversational Memory
- **Building Memory Systems**
  - Short-term memory (conversation buffer)
  - Long-term memory (persistent storage)
  - Memory retrieval strategies
  - Memory summarization

### 3. Context Engineering
- **Optimizing Context**
  - Context window management
  - Relevant information selection
  - Context compression techniques
  - Dynamic context loading

### 4. State Persistence
- **Storing Agent State**
  - State serialization
  - Database integration
  - File-based storage
  - Distributed state management

### 5. Multi-Turn Conversations
- **Managing Dialogue**
  - Turn tracking
  - Context continuity
  - Topic switching
  - Conversation repair

### 6. Memory Optimization
- **Performance Tuning**
  - Memory size limits
  - Pruning strategies
  - Caching mechanisms
  - Cost optimization

---

## 🛠️ Key Concepts

### Session Architecture

```python
class AgentSession:
    def __init__(self, session_id: str):
        self.session_id = session_id
        self.conversation_history = []
        self.context = {}
        self.created_at = datetime.now()
    
    def add_message(self, role: str, content: str):
        """Add message to conversation history."""
        self.conversation_history.append({
            "role": role,
            "content": content,
            "timestamp": datetime.now()
        })
    
    def get_context(self) -> str:
        """Build context from conversation history."""
        return "\n".join([
            f"{msg['role']}: {msg['content']}"
            for msg in self.conversation_history
        ])
```

### Memory Types

| Memory Type | Duration | Use Case | Storage |
|-------------|----------|----------|---------|
| **Buffer Memory** | Session only | Conversation context | In-memory |
| **Summary Memory** | Medium-term | Key points only | In-memory/DB |
| **Vector Memory** | Long-term | Semantic search | Vector DB |
| **Entity Memory** | Long-term | User preferences | Database |

### Context Window Management

```python
def manage_context_window(messages, max_tokens=4000):
    """Keep conversation within token limits."""
    total_tokens = 0
    kept_messages = []
    
    # Keep most recent messages
    for msg in reversed(messages):
        msg_tokens = count_tokens(msg)
        if total_tokens + msg_tokens > max_tokens:
            break
        kept_messages.insert(0, msg)
        total_tokens += msg_tokens
    
    return kept_messages
```

---

## 🏗️ Memory Architectures

### Architecture 1: Simple Buffer Memory

```
User Input → Agent → Response
              ↓
         Conversation
            Buffer
```

**Best for**: Short conversations, simple context.

### Architecture 2: Hybrid Memory System

```
User Input → Agent → Response
              ↓
         ┌────┴────┐
    Buffer      Vector
    Memory      Memory
         └────┬────┘
          Synthesized
           Context
```

**Best for**: Long conversations, semantic search.

### Architecture 3: Multi-Agent Shared Memory

```
User → Orchestrator Agent
         ↓
    ┌────┼────┐
Agent A  Agent B  Agent C
    └────┼────┘
      Shared
     Memory Store
```

**Best for**: Complex workflows, agent collaboration.

---

## 💡 Practical Examples

### Example 1: Personal Assistant with Memory

Build an agent that remembers user preferences and context.

**Features:**
- Conversation history
- User preferences storage
- Context-aware responses
- Long-term memory

**Memory Components:**
```python
class PersonalAssistant:
    def __init__(self, user_id):
        self.session = Session(user_id)
        self.preferences = load_preferences(user_id)
        self.conversation_history = []
    
    async def chat(self, message):
        # Add context from memory
        context = self.build_context()
        
        # Get response with context
        response = await agent.query(message, context=context)
        
        # Update memory
        self.conversation_history.append({
            "user": message,
            "assistant": response
        })
        
        return response
```

### Example 2: Multi-Session Customer Support

Create an agent that handles multiple customer sessions.

**Features:**
- Session tracking
- Issue history
- Transfer between agents
- State persistence

**Session Management:**
```python
class SupportAgent:
    def __init__(self):
        self.sessions = {}
    
    def get_or_create_session(self, customer_id):
        if customer_id not in self.sessions:
            self.sessions[customer_id] = Session(
                customer_id,
                history=load_history(customer_id)
            )
        return self.sessions[customer_id]
```

### Example 3: Research Agent with Knowledge Base

Build an agent that accumulates research findings.

**Features:**
- Research session tracking
- Finding accumulation
- Source attribution
- Summary generation

**Knowledge Management:**
```python
class ResearchAgent:
    def __init__(self):
        self.knowledge_base = VectorStore()
        self.current_research = {}
    
    async def research(self, topic):
        # Check existing knowledge
        existing = self.knowledge_base.search(topic)
        
        # Research and add new findings
        new_findings = await self.find_information(topic)
        self.knowledge_base.add(new_findings)
        
        # Generate summary
        return self.summarize(existing + new_findings)
```

---

## 🚀 Getting Started

### Prerequisites

```bash
# Install required packages
pip install google-adk chromadb sqlalchemy redis
```

### Basic Session Setup

```python
from google import adk
from adk.sessions import Session

# Create a session
session = Session(
    id="user_123",
    max_history=10,
    memory_type="buffer"
)

# Use session with agent
agent = adk.Agent(
    model="gemini-pro",
    session=session
)

# Chat with memory
response1 = await agent.chat("My name is Alice")
response2 = await agent.chat("What's my name?")  # Remembers!
```

---

## 🎓 Exercises

### Exercise 1: Build a Shopping Assistant

Create an agent that remembers:
- Shopping cart contents
- User preferences
- Previous purchases
- Budget constraints

**Challenge**: Implement cart persistence across sessions.

### Exercise 2: Learning Tutor Agent

Build an agent that tracks:
- Student progress
- Learning history
- Weak areas
- Study patterns

**Challenge**: Adapt teaching style based on history.

### Exercise 3: Project Management Agent

Create an agent that manages:
- Project status
- Task assignments
- Team member context
- Decision history

**Challenge**: Share context between team members.

---

## 💾 Storage Solutions

### In-Memory Storage

```python
class MemoryStore:
    def __init__(self):
        self.store = {}
    
    def save(self, session_id, data):
        self.store[session_id] = data
    
    def load(self, session_id):
        return self.store.get(session_id, {})
```

**Pros**: Fast, simple  
**Cons**: Not persistent, limited scale

### Database Storage

```python
from sqlalchemy import create_engine, Column, String, JSON
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class SessionState(Base):
    __tablename__ = 'sessions'
    
    session_id = Column(String, primary_key=True)
    state = Column(JSON)
```

**Pros**: Persistent, queryable  
**Cons**: Slower, requires setup

### Vector Database (for Semantic Search)

```python
from chromadb import Client

client = Client()
collection = client.create_collection("agent_memory")

# Store memories with embeddings
collection.add(
    documents=["User prefers morning meetings"],
    metadatas=[{"user_id": "123", "type": "preference"}],
    ids=["pref_1"]
)

# Search semantically
results = collection.query(
    query_texts=["meeting times"],
    n_results=5
)
```

**Pros**: Semantic search, scalable  
**Cons**: Complex setup, cost

---

## 📊 Performance Optimization

### Memory Pruning Strategy

```python
def prune_conversation(history, max_messages=20):
    """Keep only recent messages."""
    if len(history) <= max_messages:
        return history
    
    # Always keep first message (context)
    return [history[0]] + history[-(max_messages-1):]
```

### Conversation Summarization

```python
async def summarize_conversation(history):
    """Summarize old conversation parts."""
    if len(history) < 10:
        return history
    
    # Summarize older messages
    old_messages = history[:-5]
    summary = await agent.summarize(old_messages)
    
    # Keep summary + recent messages
    return [{"role": "system", "content": summary}] + history[-5:]
```

### Lazy Loading

```python
class LazyMemory:
    def __init__(self, session_id):
        self.session_id = session_id
        self._memory = None
    
    @property
    def memory(self):
        if self._memory is None:
            self._memory = load_from_database(self.session_id)
        return self._memory
```

---

## 🔍 Context Engineering Patterns

### Sliding Window

Keep only the N most recent messages.

```python
context = conversation_history[-10:]
```

### Summarization + Recent

Summarize old context, keep recent messages.

```python
context = [summary_of_old_messages] + recent_messages
```

### Semantic Filtering

Retrieve only relevant past messages.

```python
relevant = vector_db.search(current_query, top_k=5)
context = relevant + recent_messages
```

### Entity-Based

Focus on mentioned entities.

```python
entities = extract_entities(conversation)
context = get_context_for_entities(entities)
```

---

## 📖 Key Takeaways

- 🔑 **Sessions enable** stateful, context-aware agents
- 🔑 **Memory types** serve different use cases and durations
- 🔑 **Context management** is critical for token efficiency
- 🔑 **Persistence strategy** depends on scale and requirements
- 🔑 **Memory optimization** reduces costs and improves performance
- 🔑 **Shared memory** enables multi-agent collaboration

---

## 🔗 Additional Resources

### Documentation
- [ADK Sessions Guide](https://google.github.io/adk-docs/sessions)
- [Memory Management Best Practices](https://google.github.io/adk-docs/memory)
- [Context Engineering](https://ai.google.dev/gemini-api/docs/context)

### Tools & Libraries
- [ChromaDB](https://www.trychroma.com/) - Vector database
- [Redis](https://redis.io/) - In-memory store
- [SQLAlchemy](https://www.sqlalchemy.org/) - Database toolkit

### Research Papers
- [Memory Mechanisms in LLMs](https://arxiv.org/abs/2304.03442)
- [Context Window Optimization](https://arxiv.org/abs/2305.14283)

---

## ❓ Common Issues & Solutions

<details>
<summary><b>Context Window Exceeded</b></summary>

**Problem**: Conversation too long for model

**Solution**:
- Implement conversation summarization
- Use sliding window approach
- Store only relevant context
- Consider using models with larger windows

</details>

<details>
<summary><b>Memory Growing Too Large</b></summary>

**Problem**: Memory consumption increasing over time

**Solution**:
- Implement pruning strategy
- Use summarization
- Archive old sessions
- Set memory limits

</details>

<details>
<summary><b>State Inconsistency</b></summary>

**Problem**: State not syncing across instances

**Solution**:
- Use centralized storage (Redis/DB)
- Implement proper locking
- Add state versioning
- Use distributed transactions

</details>

<details>
<summary><b>Slow Memory Retrieval</b></summary>

**Problem**: Loading session state is slow

**Solution**:
- Implement caching layer
- Use lazy loading
- Optimize database queries
- Consider in-memory store

</details>

---

## 🎯 Congratulations! 🎉

You've completed the 5-Day AI Agents Intensive Course!

### What You've Learned

✅ Built AI agents from scratch  
✅ Implemented function calling and tool use  
✅ Integrated MCP servers  
✅ Handled long-running operations  
✅ Managed sessions and memory  
✅ Applied production best practices  

### Next Steps

1. **Build Your Own Agent** - Apply what you've learned
2. **Explore Advanced Topics** - Dive deeper into specific areas
3. **Join the Community** - Share your projects
4. **Keep Learning** - AI agents are evolving rapidly!

---

## 🏆 Project Ideas

Ready to build something? Try these:

### Beginner Projects
- ⭐ Personal task manager agent
- ⭐ Weather and news briefing agent
- ⭐ Simple Q&A assistant

### Intermediate Projects
- ⭐⭐ Code review agent
- ⭐⭐ Research assistant with sources
- ⭐⭐ Customer support chatbot

### Advanced Projects
- ⭐⭐⭐ Multi-agent research system
- ⭐⭐⭐ Autonomous workflow automation
- ⭐⭐⭐ Enterprise knowledge assistant

---

<div align="center">

[← Day 2](../Day-2) | [Back to Main](../README.md)

**Thank you for learning with us!** 🙏

[![Share](https://img.shields.io/badge/Share-Your%20Project-blue?style=for-the-badge)](https://github.com)
[![Star](https://img.shields.io/badge/Star-This%20Repo-yellow?style=for-the-badge)](https://github.com)

</div>
