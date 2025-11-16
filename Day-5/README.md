# 🤝 Day 5: Agent-to-Agent Communication

<div align="center">

![Day 5](https://img.shields.io/badge/Day-5-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Expert-darkred?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-3%20hours-orange?style=for-the-badge)

**Build Multi-Agent Systems and Collaborative Workflows**

[← Day 4](../Day-4) | [Back to Main](../README.md)

</div>

---

## 🎯 Learning Objectives

By the end of this day, you will:

- ✅ Design and implement multi-agent architectures
- ✅ Enable communication between multiple agents
- ✅ Orchestrate complex multi-agent workflows
- ✅ Implement task delegation and specialization
- ✅ Handle agent collaboration and conflict resolution
- ✅ Build hierarchical agent systems
- ✅ Deploy production multi-agent applications

---

## 📓 Notebook

**[`day-5a-agent2agent-communication.ipynb`](./day-5a-agent2agent-communication.ipynb)**

This notebook covers advanced multi-agent systems and inter-agent communication patterns.

---

## 📚 Topics Covered

### 1. Multi-Agent Architecture Patterns
- **Design Principles**
  - When to use multiple agents
  - Agent specialization vs generalization
  - Communication protocols
  - Coordination strategies

### 2. Inter-Agent Communication
- **Communication Methods**
  - Direct messaging
  - Shared state/memory
  - Event-driven communication
  - Message queues and brokers

### 3. Agent Orchestration
- **Coordination Patterns**
  - Central orchestrator
  - Peer-to-peer collaboration
  - Hierarchical delegation
  - Swarm intelligence

### 4. Task Delegation
- **Work Distribution**
  - Task decomposition
  - Agent capability matching
  - Load balancing
  - Result aggregation

### 5. Conflict Resolution
- **Managing Disagreements**
  - Consensus mechanisms
  - Priority-based resolution
  - Human arbitration
  - Voting systems

### 6. Collaborative Workflows
- **Building Complex Systems**
  - Sequential workflows
  - Parallel execution
  - Conditional branching
  - Error recovery

---

## 🛠️ Key Concepts

### Multi-Agent Architecture Types

```python
# 1. ORCHESTRATOR PATTERN
class OrchestratorAgent:
    def __init__(self):
        self.specialist_agents = {
            "research": ResearchAgent(),
            "analysis": AnalysisAgent(),
            "writing": WritingAgent()
        }
    
    async def process(self, task):
        """Coordinate multiple agents."""
        # Decompose task
        subtasks = self.decompose_task(task)
        
        # Delegate to specialists
        results = []
        for subtask in subtasks:
            agent = self.select_agent(subtask)
            result = await agent.process(subtask)
            results.append(result)
        
        # Aggregate results
        return self.aggregate(results)

# 2. PEER-TO-PEER PATTERN
class CollaborativeAgent:
    def __init__(self, peers):
        self.peers = peers
    
    async def collaborate(self, task):
        """Work with peer agents."""
        # Broadcast task
        responses = await asyncio.gather(*[
            peer.request_input(task)
            for peer in self.peers
        ])
        
        # Reach consensus
        return self.consensus(responses)

# 3. HIERARCHICAL PATTERN
class ManagerAgent:
    def __init__(self):
        self.teams = {
            "team_a": [Agent1(), Agent2()],
            "team_b": [Agent3(), Agent4()]
        }
    
    async def delegate(self, task):
        """Delegate to appropriate team."""
        team = self.select_team(task)
        team_lead = team[0]
        
        # Team lead coordinates team members
        return await team_lead.coordinate(task, team)
```

### Communication Protocols

| Protocol | Use Case | Pros | Cons |
|----------|----------|------|------|
| **Direct Call** | Simple coordination | Fast, simple | Tight coupling |
| **Message Queue** | Async workflows | Decoupled, scalable | Complexity |
| **Shared Memory** | State sharing | Fast access | Race conditions |
| **Event Bus** | Loose coupling | Flexible | Debugging harder |

### Agent Communication Example

```python
from dataclasses import dataclass
from typing import Any, Dict
from enum import Enum

class MessageType(Enum):
    REQUEST = "request"
    RESPONSE = "response"
    NOTIFICATION = "notification"

@dataclass
class AgentMessage:
    """Standard message format between agents."""
    sender: str
    receiver: str
    message_type: MessageType
    content: Dict[str, Any]
    correlation_id: str
    timestamp: datetime

class CommunicationBus:
    """Central message bus for agent communication."""
    
    def __init__(self):
        self.agents = {}
        self.message_queue = asyncio.Queue()
    
    def register_agent(self, agent_id: str, agent):
        """Register an agent with the bus."""
        self.agents[agent_id] = agent
    
    async def send_message(self, message: AgentMessage):
        """Send message to target agent."""
        await self.message_queue.put(message)
    
    async def process_messages(self):
        """Process messages from queue."""
        while True:
            message = await self.message_queue.get()
            
            if message.receiver in self.agents:
                receiver = self.agents[message.receiver]
                await receiver.handle_message(message)

# Usage
bus = CommunicationBus()

class ResearchAgent:
    def __init__(self, agent_id, bus):
        self.id = agent_id
        self.bus = bus
        bus.register_agent(agent_id, self)
    
    async def handle_message(self, message: AgentMessage):
        """Handle incoming messages."""
        if message.message_type == MessageType.REQUEST:
            # Process request
            result = await self.research(message.content["query"])
            
            # Send response
            response = AgentMessage(
                sender=self.id,
                receiver=message.sender,
                message_type=MessageType.RESPONSE,
                content={"result": result},
                correlation_id=message.correlation_id,
                timestamp=datetime.now()
            )
            await self.bus.send_message(response)
```

---

## 🏗️ Multi-Agent Architectures

### Architecture 1: Orchestrated Specialists

```
                    User Request
                         ↓
                   Orchestrator
                         ↓
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
   Research Agent   Analysis Agent   Writing Agent
        ↓                ↓                ↓
        └────────────────┼────────────────┘
                         ↓
                 Aggregated Result
```

**Best for**: Complex tasks requiring different skills, clear task decomposition.

### Architecture 2: Collaborative Peers

```
    Agent A ←→ Agent B
       ↕          ↕
    Agent C ←→ Agent D
```

**Best for**: Flexible collaboration, distributed decision-making.

### Architecture 3: Hierarchical Organization

```
              Executive Agent
                     ↓
        ┌────────────┴────────────┐
        ↓                         ↓
   Manager Agent A          Manager Agent B
        ↓                         ↓
   ┌────┴────┐              ┌────┴────┐
Worker A1  Worker A2     Worker B1  Worker B2
```

**Best for**: Large-scale systems, clear hierarchy, scalability.

### Architecture 4: Pipeline Chain

```
Input → Agent 1 → Agent 2 → Agent 3 → Output
         (Parse)   (Process)  (Format)
```

**Best for**: Sequential processing, data transformation pipelines.

---

## 💡 Practical Examples

### Example 1: Research & Writing Team

Build a team of agents that collaborates on content creation.

**Agents:**
- **Research Agent**: Gathers information
- **Analysis Agent**: Analyzes findings
- **Writing Agent**: Creates content
- **Editor Agent**: Reviews and refines

**Implementation:**
```python
class ContentCreationTeam:
    def __init__(self):
        self.researcher = ResearchAgent()
        self.analyzer = AnalysisAgent()
        self.writer = WritingAgent()
        self.editor = EditorAgent()
    
    async def create_article(self, topic):
        """Collaborative article creation."""
        
        # Phase 1: Research
        research_results = await self.researcher.research(topic)
        
        # Phase 2: Analysis
        analysis = await self.analyzer.analyze(research_results)
        
        # Phase 3: Writing
        draft = await self.writer.write(topic, analysis)
        
        # Phase 4: Editing
        final = await self.editor.edit(draft)
        
        return final

# Usage
team = ContentCreationTeam()
article = await team.create_article("AI Agents in Production")
```

### Example 2: Customer Support System

Multi-agent system handling customer inquiries.

**Agents:**
- **Triage Agent**: Categorizes inquiries
- **Technical Agent**: Handles technical issues
- **Billing Agent**: Handles billing questions
- **Escalation Agent**: Manages complex cases

**Implementation:**
```python
class CustomerSupportSystem:
    def __init__(self):
        self.triage = TriageAgent()
        self.specialists = {
            "technical": TechnicalAgent(),
            "billing": BillingAgent(),
            "general": GeneralAgent()
        }
        self.escalation = EscalationAgent()
    
    async def handle_inquiry(self, inquiry):
        """Route and handle customer inquiry."""
        
        # Classify inquiry
        category = await self.triage.classify(inquiry)
        
        # Route to specialist
        specialist = self.specialists.get(category)
        
        try:
            response = await specialist.respond(inquiry)
            
            # Check if satisfactory
            if not response.is_satisfactory:
                # Escalate
                response = await self.escalation.handle(
                    inquiry, 
                    previous_response=response
                )
            
            return response
            
        except Exception as e:
            # Auto-escalate on error
            return await self.escalation.handle(
                inquiry, 
                error=e
            )

# With confidence scoring
class TriageAgent:
    async def classify(self, inquiry):
        """Classify inquiry with confidence."""
        result = await self.model.classify(inquiry)
        
        if result.confidence < 0.7:
            # Get second opinion from multiple agents
            opinions = await asyncio.gather(*[
                agent.classify(inquiry)
                for agent in self.backup_agents
            ])
            
            # Use voting
            result = self.vote(opinions)
        
        return result.category
```

### Example 3: Software Development Team

Agents working together on code generation.

**Agents:**
- **Architect Agent**: Designs system
- **Developer Agent**: Writes code
- **Test Agent**: Creates tests
- **Reviewer Agent**: Reviews code

**Implementation:**
```python
class SoftwareDevTeam:
    def __init__(self):
        self.architect = ArchitectAgent()
        self.developer = DeveloperAgent()
        self.tester = TestAgent()
        self.reviewer = ReviewerAgent()
    
    async def build_feature(self, requirement):
        """Collaborative feature development."""
        
        # Design phase
        design = await self.architect.design(requirement)
        
        # Implementation phase
        code = await self.developer.implement(design)
        
        # Testing phase
        tests = await self.tester.create_tests(code, requirement)
        test_results = await self.tester.run_tests(code, tests)
        
        # Review phase
        review = await self.reviewer.review(code, design)
        
        # Iterate if needed
        if not review.approved:
            code = await self.developer.fix_issues(
                code, 
                review.issues
            )
            # Retest
            test_results = await self.tester.run_tests(code, tests)
        
        return {
            "code": code,
            "tests": tests,
            "review": review,
            "test_results": test_results
        }

# With parallel execution
async def build_feature_parallel(self, requirement):
    """Build feature with parallel tasks."""
    
    # Phase 1: Design (sequential)
    design = await self.architect.design(requirement)
    
    # Phase 2: Parallel implementation and test creation
    code, tests = await asyncio.gather(
        self.developer.implement(design),
        self.tester.create_test_plan(design)
    )
    
    # Phase 3: Parallel testing and review
    test_results, review = await asyncio.gather(
        self.tester.run_tests(code, tests),
        self.reviewer.review(code, design)
    )
    
    return {
        "code": code,
        "tests": tests,
        "review": review,
        "test_results": test_results
    }
```

---

## 🚀 Getting Started

### Prerequisites

```bash
# Install required packages
pip install google-adk asyncio aiohttp redis
```

### Basic Multi-Agent Setup

```python
from google import adk
import asyncio

# Create specialized agents
research_agent = adk.Agent(
    model="gemini-pro",
    tools=[web_search, document_reader],
    system_prompt="You are a research specialist."
)

writing_agent = adk.Agent(
    model="gemini-pro",
    tools=[grammar_check, style_guide],
    system_prompt="You are a professional writer."
)

# Orchestrator
class Orchestrator:
    def __init__(self):
        self.research_agent = research_agent
        self.writing_agent = writing_agent
    
    async def create_content(self, topic):
        # Get research
        research = await self.research_agent.process(
            f"Research: {topic}"
        )
        
        # Create content
        content = await self.writing_agent.process(
            f"Write about {topic} using: {research}"
        )
        
        return content

# Run
orchestrator = Orchestrator()
result = await orchestrator.create_content("AI Agents")
```

---

## 🎓 Exercises

### Exercise 1: Build a News Team

Create a multi-agent news system:
- **Reporter**: Gathers news
- **Fact-Checker**: Verifies information
- **Editor**: Formats and publishes

**Challenge**: Add real-time collaboration.

### Exercise 2: Create a Data Analysis Pipeline

Build agents for data processing:
- **Loader**: Loads data sources
- **Cleaner**: Cleans and validates
- **Analyzer**: Performs analysis
- **Visualizer**: Creates charts

**Challenge**: Handle failures and retries.

### Exercise 3: Build a Planning System

Create agents for project planning:
- **Estimator**: Estimates effort
- **Scheduler**: Creates timeline
- **Resource Manager**: Assigns resources
- **Risk Analyzer**: Identifies risks

**Challenge**: Implement consensus-based decisions.

---

## 🔄 Communication Patterns

### Pattern 1: Request-Response

```python
async def request_response():
    """Simple request-response pattern."""
    request = {
        "task": "analyze_data",
        "data": dataset
    }
    
    response = await agent.send_request(request)
    return response
```

### Pattern 2: Publish-Subscribe

```python
class EventBus:
    def __init__(self):
        self.subscribers = defaultdict(list)
    
    def subscribe(self, event_type, handler):
        """Subscribe to event type."""
        self.subscribers[event_type].append(handler)
    
    async def publish(self, event_type, data):
        """Publish event to all subscribers."""
        handlers = self.subscribers[event_type]
        await asyncio.gather(*[
            handler(data) for handler in handlers
        ])

# Usage
bus = EventBus()

# Agents subscribe to events
bus.subscribe("data_ready", analysis_agent.process)
bus.subscribe("data_ready", visualization_agent.process)

# Publish event
await bus.publish("data_ready", dataset)
```

### Pattern 3: Workflow Chain

```python
class WorkflowChain:
    def __init__(self, agents):
        self.agents = agents
    
    async def execute(self, input_data):
        """Execute agents in sequence."""
        data = input_data
        
        for agent in self.agents:
            data = await agent.process(data)
            
            # Early exit on error
            if data.has_error:
                return data
        
        return data

# Usage
workflow = WorkflowChain([
    parse_agent,
    validate_agent,
    process_agent,
    format_agent
])

result = await workflow.execute(raw_input)
```

### Pattern 4: Map-Reduce

```python
async def map_reduce(data_chunks, agents):
    """Parallel processing with aggregation."""
    
    # Map: Process chunks in parallel
    results = await asyncio.gather(*[
        agent.process(chunk)
        for chunk, agent in zip(data_chunks, agents)
    ])
    
    # Reduce: Aggregate results
    final_result = aggregate(results)
    
    return final_result
```

---

## 🎭 Agent Specialization

### Defining Agent Roles

```python
class AgentRole:
    """Define agent capabilities and responsibilities."""
    
    def __init__(self, name, capabilities, tools):
        self.name = name
        self.capabilities = capabilities
        self.tools = tools
        self.workload = 0

# Create specialized roles
roles = {
    "researcher": AgentRole(
        name="Researcher",
        capabilities=["web_search", "data_collection"],
        tools=[search_tool, scraper_tool]
    ),
    "analyst": AgentRole(
        name="Analyst",
        capabilities=["data_analysis", "visualization"],
        tools=[pandas_tool, plot_tool]
    ),
    "writer": AgentRole(
        name="Writer",
        capabilities=["content_creation", "editing"],
        tools=[grammar_tool, style_tool]
    )
}

# Task routing based on capabilities
def route_task(task, roles):
    """Route task to agent with matching capabilities."""
    required_capability = task.required_capability
    
    # Find agents with capability
    candidates = [
        role for role in roles.values()
        if required_capability in role.capabilities
    ]
    
    # Select least loaded
    return min(candidates, key=lambda r: r.workload)
```

---

## ⚖️ Conflict Resolution

### Voting System

```python
class VotingSystem:
    async def get_consensus(self, agents, question):
        """Get consensus through voting."""
        
        # Collect votes
        votes = await asyncio.gather(*[
            agent.vote(question) for agent in agents
        ])
        
        # Count votes
        vote_counts = Counter(votes)
        winner, count = vote_counts.most_common(1)[0]
        
        # Require majority
        if count > len(agents) / 2:
            return winner
        else:
            # No consensus, escalate to human
            return await self.request_human_decision(
                question, 
                votes
            )
```

### Priority-Based Resolution

```python
class PriorityResolver:
    def __init__(self):
        self.agent_priorities = {
            "expert_agent": 10,
            "senior_agent": 5,
            "junior_agent": 1
        }
    
    def resolve(self, conflicting_responses):
        """Resolve based on agent priority."""
        
        # Sort by priority
        sorted_responses = sorted(
            conflicting_responses,
            key=lambda r: self.agent_priorities[r.agent_id],
            reverse=True
        )
        
        # Return highest priority response
        return sorted_responses[0]
```

### Confidence-Based Selection

```python
async def confidence_based_selection(agents, task):
    """Select response with highest confidence."""
    
    # Get responses with confidence scores
    responses = await asyncio.gather(*[
        agent.process_with_confidence(task)
        for agent in agents
    ])
    
    # Select highest confidence
    best_response = max(
        responses, 
        key=lambda r: r.confidence
    )
    
    # Verify confidence threshold
    if best_response.confidence > 0.8:
        return best_response
    else:
        # Low confidence, get human input
        return await request_human_review(responses)
```

---

## 📊 Performance Optimization

### Load Balancing

```python
class LoadBalancer:
    def __init__(self, agents):
        self.agents = agents
        self.load = {agent.id: 0 for agent in agents}
    
    def get_available_agent(self):
        """Get least loaded agent."""
        return min(
            self.agents,
            key=lambda a: self.load[a.id]
        )
    
    async def distribute_task(self, task):
        """Distribute task to available agent."""
        agent = self.get_available_agent()
        
        self.load[agent.id] += 1
        try:
            result = await agent.process(task)
            return result
        finally:
            self.load[agent.id] -= 1
```

### Caching Shared Results

```python
from functools import lru_cache

class SharedCache:
    def __init__(self):
        self.cache = {}
    
    async def get_or_compute(self, key, compute_func):
        """Get from cache or compute."""
        if key not in self.cache:
            self.cache[key] = await compute_func()
        return self.cache[key]

# Usage with multiple agents
shared_cache = SharedCache()

async def agent_with_cache(query):
    """Agent using shared cache."""
    cache_key = hash(query)
    
    result = await shared_cache.get_or_compute(
        cache_key,
        lambda: agent.process(query)
    )
    
    return result
```

---

## 📖 Key Takeaways

- 🔑 **Multi-agent systems** enable complex problem solving
- 🔑 **Communication patterns** determine system architecture
- 🔑 **Specialization** improves efficiency and quality
- 🔑 **Orchestration** coordinates agent collaboration
- 🔑 **Conflict resolution** ensures consistent outcomes
- 🔑 **Load balancing** optimizes resource usage

---

## 🔗 Additional Resources

### Documentation
- [ADK Multi-Agent Patterns](https://google.github.io/adk-docs/multi-agent)
- [Distributed Systems Patterns](https://www.oreilly.com/library/view/designing-distributed-systems/9781491983638/)

### Research Papers
- [Multi-Agent Systems](https://arxiv.org/abs/2308.08155)
- [Agent Communication Languages](https://www.sciencedirect.com/topics/computer-science/agent-communication-language)

### Tools
- [Ray](https://www.ray.io/) - Distributed computing
- [Celery](https://docs.celeryproject.org/) - Task queue
- [RabbitMQ](https://www.rabbitmq.com/) - Message broker

---

## ❓ Common Issues & Solutions

<details>
<summary><b>Agent Deadlock</b></summary>

**Problem**: Agents waiting for each other

**Solution**:
- Implement timeouts
- Use async patterns
- Add circuit breakers
- Design acyclic dependencies

</details>

<details>
<summary><b>Inconsistent State</b></summary>

**Problem**: Agents have different views of state

**Solution**:
- Use centralized state store
- Implement event sourcing
- Add state versioning
- Use distributed locks

</details>

<details>
<summary><b>Message Loss</b></summary>

**Problem**: Messages not reaching agents

**Solution**:
- Add message acknowledgments
- Implement retry logic
- Use persistent queues
- Add monitoring

</details>

<details>
<summary><b>High Communication Overhead</b></summary>

**Problem**: Too much inter-agent communication

**Solution**:
- Batch messages
- Use caching
- Optimize protocols
- Reduce coupling

</details>

---

## 🎯 Congratulations! 🎉

You've completed the **5-Day AI Agents Intensive Course**!

### Your Journey

✅ **Day 1**: Built your first AI agent with function calling  
✅ **Day 2**: Mastered tools, MCP, and best practices  
✅ **Day 3**: Implemented sessions and memory  
✅ **Day 4**: Added observability and monitoring  
✅ **Day 5**: Created multi-agent systems  

### What's Next?

1. **Build Real Projects** - Apply your knowledge
2. **Join Communities** - Share and learn
3. **Stay Updated** - AI agents evolve rapidly
4. **Contribute** - Help others learn

---

## 🏆 Final Project Ideas

### Beginner Level
- ⭐ Multi-agent chatbot team
- ⭐ Automated content pipeline
- ⭐ Simple workflow automation

### Intermediate Level
- ⭐⭐ Customer support system
- ⭐⭐ Code review team
- ⭐⭐ Research assistant network

### Advanced Level
- ⭐⭐⭐ Enterprise automation platform
- ⭐⭐⭐ Distributed agent swarm
- ⭐⭐⭐ Self-improving agent system

---

<div align="center">

[← Day 4](../Day-4) | [Back to Main](../README.md)

**Thank you for learning with us!** 🙏

### Share Your Success

[![LinkedIn](https://img.shields.io/badge/Share%20on-LinkedIn-blue?style=for-the-badge&logo=linkedin)](https://linkedin.com)
[![Twitter](https://img.shields.io/badge/Share%20on-Twitter-blue?style=for-the-badge&logo=twitter)](https://twitter.com)
[![Star](https://img.shields.io/badge/Star-This%20Repo-yellow?style=for-the-badge&logo=github)](https://github.com)

**Keep Building!** 🚀

</div>
