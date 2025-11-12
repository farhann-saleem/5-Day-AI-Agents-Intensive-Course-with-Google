# 📘 Day 1: From Prompt to Action

<div align="center">

![Day 1](https://img.shields.io/badge/Day-1-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-2%20hours-orange?style=for-the-badge)

**Introduction to AI Agents and Function Calling**

[← Back to Main](../README.md) | [Next Day →](../Day-2)

</div>

---

## 🎯 Learning Objectives

By the end of this day, you will:

- ✅ Understand the difference between LLMs and AI agents
- ✅ Set up Google's Agent Development Kit (ADK)
- ✅ Implement your first function calling with Gemini
- ✅ Build a simple tool-using agent
- ✅ Handle agent responses and manage errors

---

## 📓 Notebook

**[`day-1a-from-prompt-to-action.ipynb`](./day-1a-from-prompt-to-action.ipynb)**

This notebook covers the fundamentals of building AI agents that can take actions.

---

## 📚 Topics Covered

### 1. Introduction to AI Agents
- **What is an AI Agent?**
  - Definition and core concepts
  - Difference between LLMs and agents
  - Agent architecture overview

### 2. Setting Up Your Environment
- **Getting Started with ADK**
  - Installing Google ADK
  - Configuring API credentials
  - Basic project setup

### 3. Function Calling Basics
- **Understanding Function Calling**
  - How function calling works
  - Function schemas and definitions
  - Parameter passing and validation

### 4. Building Your First Agent
- **Hands-On Implementation**
  - Defining tools and functions
  - Registering tools with the agent
  - Making agent calls
  - Processing responses

### 5. Error Handling
- **Managing Common Issues**
  - API errors and rate limits
  - Invalid function calls
  - Timeout handling
  - Best practices for resilience

---

## 🛠️ Key Concepts

### Function Calling

Function calling allows LLMs to interact with external tools and APIs:

```python
# Example: Defining a simple tool
def get_weather(location: str) -> dict:
    """Get weather information for a location."""
    # Implementation here
    return {"temp": 72, "condition": "sunny"}
```

### Agent vs LLM

| Feature | LLM | Agent |
|---------|-----|-------|
| **Interaction** | Text in → Text out | Can call functions |
| **Actions** | None | Can take actions |
| **Tools** | No tool access | Can use external tools |
| **Autonomy** | Single response | Multi-step reasoning |

### Tool Registration

Tools must be properly defined with schemas:

```python
tool_schema = {
    "name": "get_weather",
    "description": "Get current weather for a location",
    "parameters": {
        "location": {
            "type": "string",
            "description": "City name"
        }
    }
}
```

---

## 💡 Practical Examples

### Example 1: Simple Calculator Agent

Build an agent that can perform mathematical operations.

**Tools Required:**
- `add(a, b)` - Addition
- `subtract(a, b)` - Subtraction
- `multiply(a, b)` - Multiplication

### Example 2: Weather Information Agent

Create an agent that fetches weather data.

**Tools Required:**
- `get_weather(location)` - Get current weather
- `get_forecast(location, days)` - Get forecast

### Example 3: Time and Date Agent

Build an agent that can answer time-related queries.

**Tools Required:**
- `get_current_time()` - Get current time
- `get_date()` - Get current date
- `convert_timezone(time, from_tz, to_tz)` - Convert timezones

---

## 🚀 Getting Started

### Prerequisites

```bash
# Install required packages
pip install google-adk jupyter
```

### Setup

```python
import os
os.environ["GOOGLE_API_KEY"] = "your_api_key_here"

from google import adk
```

### Running the Notebook

1. Open `day-1a-from-prompt-to-action.ipynb` in Jupyter
2. Run cells sequentially
3. Follow along with the examples
4. Complete the exercises

---

## 🎓 Exercises

### Exercise 1: Build a Unit Converter Agent
Create an agent that can convert between different units (temperature, distance, weight).

**Challenge**: Handle multiple unit types.

### Exercise 2: Text Analysis Agent
Build an agent that can analyze text (word count, sentiment, language detection).

**Challenge**: Combine multiple tools in one query.

### Exercise 3: Date Calculator Agent
Create an agent that can calculate days between dates, add/subtract days, etc.

**Challenge**: Handle different date formats.

---

## 📖 Key Takeaways

- 🔑 **Agents extend LLMs** with the ability to take actions
- 🔑 **Function calling** is the core mechanism for tool use
- 🔑 **Proper tool definition** is critical for agent success
- 🔑 **Error handling** ensures robust agent behavior
- 🔑 **Start simple** and gradually add complexity

---

## 🔗 Additional Resources

### Documentation
- [ADK Function Calling Guide](https://google.github.io/adk-docs/function-calling)
- [Gemini API: Function Calling](https://ai.google.dev/gemini-api/docs/function-calling)

### Tutorials
- [Building Your First Agent](https://google.github.io/adk-docs/tutorials/first-agent)
- [Function Calling Best Practices](https://ai.google.dev/gemini-api/docs/function-calling/best-practices)

### Examples
- [ADK Examples Repository](https://github.com/google/adk-examples)

---

## ❓ Common Issues & Solutions

<details>
<summary><b>API Key Not Working</b></summary>

**Problem**: Authentication errors

**Solution**: 
- Verify your API key is correct
- Check it's properly set in environment variables
- Ensure the key has appropriate permissions

</details>

<details>
<summary><b>Function Not Being Called</b></summary>

**Problem**: Agent doesn't use the defined function

**Solution**:
- Check function description is clear
- Verify parameter schema is correct
- Ensure function name is descriptive

</details>

<details>
<summary><b>Rate Limit Errors</b></summary>

**Problem**: Too many API calls

**Solution**:
- Implement exponential backoff
- Add delays between calls
- Consider upgrading API tier

</details>

---

## 🎯 Next Steps

Ready to move forward? Continue to:

### [Day 2: Agent Tools & Best Practices →](../Day-2)

Learn about:
- Model Context Protocol (MCP)
- Advanced tool patterns
- Long-running operations
- Production best practices

---

<div align="center">

[← Back to Main](../README.md) | [Day 2 →](../Day-2)

**Happy Building!** 🚀

</div>
