# 🤖 5-Day AI Agents Intensive Course

<div align="center">

![AI Agents](https://img.shields.io/badge/AI-Agents-blue?style=for-the-badge)
![Google](https://img.shields.io/badge/Google-ADK-red?style=for-the-badge)
![Kaggle](https://img.shields.io/badge/Kaggle-Course-20BEFF?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.8+-green?style=for-the-badge)

**Master the art of building production-ready AI agents with Google's Agent Development Kit**

[Course Link](https://www.kaggle.com/learn-guide/5-day-agents) • [ADK Docs](https://google.github.io/adk-docs/) • [API Key](https://aistudio.google.com/app/api-keys)

</div>

---

## 📖 About This Repository

This repository contains comprehensive notebooks from **Google x Kaggle's 5-Day AI Agents Intensive Course**. The course takes you from basic prompts to building sophisticated AI agents capable of tool use, long-running operations, and maintaining conversational memory.

### 🎯 What You'll Learn

- 🔧 **Build functional AI agents** using Google's Agent Development Kit (ADK)
- 🛠️ **Implement tool calling** and function execution
- 🔄 **Handle long-running operations** with async patterns
- 💾 **Manage agent memory** and conversation state
- 🌐 **Integrate MCP servers** for extended capabilities
- 🏭 **Deploy production-ready** agent systems

---

## 📚 Course Structure

### 🌟 [Day 1: From Prompt to Action](./Day-1)

**Transform simple prompts into actionable AI agents**

Learn the fundamentals of AI agents, including how to:
- Understand the difference between LLMs and agents
- Implement function calling with Gemini
- Build your first tool-using agent
- Handle agent responses and errors

**📓 Notebook**: [`day-1a-from-prompt-to-action.ipynb`](./Day-1/day-1a-from-prompt-to-action.ipynb)

**⏱️ Duration**: ~2 hours

---

### 🔨 [Day 2: Agent Tools & Best Practices](./Day-2)

**Master advanced agent patterns and production techniques**

Dive deep into:
- Model Context Protocol (MCP) integration
- Building custom tools and MCP servers
- Long-running operations and async patterns
- Human-in-the-loop workflows
- Error handling and resilience
- Agent orchestration patterns

**📓 Notebook**: [`day-2b-agent-tools-best-practices.ipynb`](./Day-2/day-2b-agent-tools-best-practices.ipynb)

**⏱️ Duration**: ~3 hours

---

### 💾 [Day 3: Agent Sessions & Memory](./Day-3)

**Build stateful agents with persistent memory**

Explore advanced state management:
- Session lifecycle management
- Context engineering techniques
- Multi-turn conversations
- Memory optimization strategies
- State sharing between agents
- Production deployment patterns

**📓 Notebook**: [`day-3a-agent-sessions.ipynb`](./Day-3/day-3a-agent-sessions.ipynb)

**⏱️ Duration**: ~2.5 hours

---

## 🚀 Technologies & Tools

| Technology | Purpose | Documentation |
|------------|---------|---------------|
| **Google ADK** | Agent Development Framework | [Docs](https://google.github.io/adk-docs/) |
| **Gemini API** | Large Language Model | [API Reference](https://ai.google.dev/gemini-api/docs) |
| **Model Context Protocol** | Tool Integration Standard | [MCP Docs](https://modelcontextprotocol.io/) |
| **Python 3.8+** | Programming Language | [Python.org](https://www.python.org/) |
| **Jupyter Notebooks** | Interactive Development | [Jupyter](https://jupyter.org/) |

---

## ⚙️ Getting Started

### Prerequisites

- Python 3.8 or higher
- Google API Key ([Get one here](https://aistudio.google.com/app/api-keys))
- Basic understanding of Python and async programming

### Installation

```bash
# Clone this repository
git clone https://github.com/yourusername/Ai-agents.git
cd Ai-agents

# Install dependencies
pip install google-adk jupyter

# Set your API key (Linux/Mac)
export GOOGLE_API_KEY="your_api_key_here"

# Set your API key (Windows)
set GOOGLE_API_KEY=your_api_key_here

# Launch Jupyter
jupyter notebook
```

### Running the Notebooks

1. Navigate to the day folder you want to explore
2. Open the `.ipynb` notebook file
3. Run cells sequentially with `Shift + Enter`
4. Follow along with the instructions and examples

---

## 📋 Course Syllabus

<details>
<summary><b>Day 1: Foundations</b></summary>

- Introduction to AI Agents
- LLMs vs Agents: Key Differences
- Setting up Google ADK
- Your First Function Call
- Tool Definition and Registration
- Handling Tool Responses
- Error Management Basics
- Practical Exercise: Weather Agent

</details>

<details>
<summary><b>Day 2: Advanced Patterns</b></summary>

- Introduction to Model Context Protocol
- Building Custom MCP Servers
- Tool Composition Strategies
- Long-Running Operations
- Async/Await Patterns
- Human-in-the-Loop Workflows
- Agent Orchestration
- Production Best Practices
- Practical Exercise: Research Agent

</details>

<details>
<summary><b>Day 3: State & Memory</b></summary>

- Session Management Architecture
- Context Engineering Techniques
- Multi-Turn Conversation Handling
- Memory Systems Overview
- State Persistence Strategies
- Cross-Session Memory
- Performance Optimization
- Deployment Considerations
- Practical Exercise: Personal Assistant

</details>

---

## 🎓 Learning Outcomes

By completing this course, you will be able to:

✅ Design and implement production-ready AI agents  
✅ Integrate external tools using MCP  
✅ Handle complex, long-running workflows  
✅ Manage agent state and memory effectively  
✅ Deploy agents with proper error handling  
✅ Optimize agent performance and costs  
✅ Follow industry best practices  

---

## 🔗 Additional Resources

### Official Documentation
- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Gemini API Reference](https://ai.google.dev/gemini-api/docs)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)

### Community & Support
- [Kaggle Discussion Forum](https://www.kaggle.com/discussions)
- [Google AI Community](https://developers.google.com/community)
- [MCP GitHub Repository](https://github.com/modelcontextprotocol)

### Related Courses
- [Kaggle: Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning)
- [Google: Generative AI Learning Path](https://www.cloudskillsboost.google/paths)

---

## 📝 Notes

- **API Usage**: These notebooks make API calls to Google's Gemini. Be mindful of your usage and costs.
- **Rate Limits**: Free tier has rate limits. Implement proper error handling and backoff strategies.
- **API Keys**: Never commit your API keys to version control. Use environment variables.

---

## 🤝 Contributing

Found an issue or have suggestions? Contributions are welcome!

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📄 License

Course materials are property of **Google** and **Kaggle**. This repository is for educational purposes.

---

## 🙏 Acknowledgments

- **Google** for developing the Agent Development Kit
- **Kaggle** for hosting and organizing the course
- **Course Instructors** for creating comprehensive materials
- **Community Contributors** for feedback and improvements

---

<div align="center">

**Ready to build your first AI agent?** 

[Start with Day 1 →](./Day-1)

⭐ **Star this repo** if you find it helpful!

</div>
