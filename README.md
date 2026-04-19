# LangChain Conversational Agent

> A terminal-based AI agent with real-time tool calling, web search, and persistent conversational memory — built with LangChain and Python.

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat&logo=python&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-0.1+-1C3C3C?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## Overview

This project implements a production-style conversational AI agent that demonstrates real-world agentic patterns: tool selection, multi-step reasoning, memory management, and graceful error handling. The agent dynamically chooses the right tool for each user query — searching the web, evaluating math, or fetching financial data — while maintaining conversation context across exchanges.

**What makes this interesting:** The agent architecture mirrors patterns used in production AI systems at companies like Salesforce, Notion, and Glean — where LLMs orchestrate tool calls rather than answer directly.

---

## Architecture

```
User Input
    │
    ▼
AgentExecutor (LangChain)
    │
    ├── Tool Router (LLM decides which tool to invoke)
    │       ├── search_tool      → Tavily API (real-time web search)
    │       ├── math_tool        → Safe expression evaluator
    │       └── ticker_tool      → Stock data lookup
    │
    ├── Conversational Memory (last 10 messages, sliding window)
    │
    └── Response Formatter → Terminal output via Rich
```

---

## Tech Stack

| Component | Technology |
|---|---|
| Agent Framework | LangChain 0.1+ |
| LLM Backend | GPT-3.5-turbo via OpenRouter |
| Web Search | Tavily API |
| Language | Python 3.9+ |
| Terminal UI | Rich library |

---

## Key Features

- **Multi-tool agent** — LLM dynamically selects the right tool per query (search, math, or custom)
- **Sliding window memory** — Maintains last 10 messages as conversation context; older messages pruned automatically
- **Safe math evaluation** — Regex-sanitized expression parser prevents code injection
- **Real-time web search** — Tavily integration returns live results with source URLs
- **Graceful error handling** — Agent recovers from API failures without crashing the loop
- **Clean CLI interface** — Rich-powered terminal output with color-coded roles

---

## Project Structure

```
langchain-conversational-agent/
├── agent.py        # Agent init, LLM binding, memory management
├── tools.py        # Tool definitions: search, math, ticker
├── main.py         # Entry point — while-loop CLI interface
├── .env.example    # Template for required API keys
├── .gitignore      # Excludes .env and secrets
└── README.md
```

---

## Setup & Run

### 1. Clone the repository
```bash
git clone https://github.com/JD1359/langchain-conversational-agent.git
cd langchain-conversational-agent
```

### 2. Install dependencies
```bash
pip install langchain langchain-openai langchain-community tavily-python python-dotenv rich
```

### 3. Configure environment variables
```bash
cp .env.example .env
# Edit .env and add your API keys
```

Required keys:
```
OPENROUTER_API_KEY=sk-or-v1-your-key-here
TAVILY_API_KEY=tvly-your-key-here
```

Get keys from: [OpenRouter](https://openrouter.ai) · [Tavily](https://app.tavily.com)

### 4. Run the agent
```bash
python main.py
```

---

## Example Interactions

```
You: What are the latest AI announcements from Google this week?
Agent: [Searches web via Tavily] Google announced...

You: Calculate compound interest: 10000 * (1 + 0.07) ** 10
Agent: Result: 19671.51

You: What was that first question I asked?
Agent: You asked about the latest AI announcements from Google this week.
```

---

## Design Decisions

**Why OpenRouter instead of OpenAI directly?**
OpenRouter provides a unified API across multiple LLM providers, allowing the agent to switch models (Claude, Gemini, Llama) without code changes — a pattern used in production multi-model systems.

**Why sliding window memory instead of vector store?**
For a terminal agent, simplicity and speed matter more than long-term recall. A 10-message sliding window handles 95% of conversational use cases with zero infrastructure overhead.

**Why Tavily over SerpAPI?**
Tavily returns structured search results (title, content, URL) natively — no HTML scraping required — reducing post-processing complexity.

---

## Future Improvements

- [ ] Persistent memory with SQLite for cross-session recall
- [ ] Add weather and news tools
- [ ] Streaming token output for real-time responses
- [ ] Web UI with Gradio or Streamlit
- [ ] Unit tests for each tool with mocked API responses

---

## Author

**Jayadeep Gopinath**
M.S. Computer Science · Illinois Institute of Technology, Chicago
[LinkedIn](https://linkedin.com/in/jayadeep-g-05b643257) · jg@hawk.illinoistech.edu
