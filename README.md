# Agentwriter-AI

An advanced AI-powered technical writing engine that transforms a single topic prompt into a structured, research-aware blog article. The system combines LangGraph orchestration, FastAPI streaming, local LLM inference via Ollama, web research, and article assembly into a polished markdown generation pipeline.

<p align="center">
  <img alt="Agentwriter-AI architecture" src="https://img.shields.io/badge/AI-Agent%20Writer-Production%20Ready-0A84FF?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="LangGraph" src="https://img.shields.io/badge/LangGraph-Orchestration-4B8BBE?style=for-the-badge" />
  <img alt="FastAPI" src="https://img.shields.io/badge/FastAPI-API%20Layer-009688?style=for-the-badge" />
  <img alt="Ollama" src="https://img.shields.io/badge/Ollama-Local%20LLM-FF6F61?style=for-the-badge" />
</p>

## Why this project matters

Most AI writing tools stop at generating a paragraph or a rough draft. Agentwriter-AI goes further by treating article production as a structured workflow:

- it decides whether a topic needs fresh research
- it builds a coherent technical outline
- it writes each section with a focused objective
- it assembles the output into a final blog
- it can plan and place technical visuals when relevant

This makes it well suited for technical content teams, research-driven blogs, internal knowledge publishing, and prototype AI editorial systems.

## Product overview

Agentwriter-AI turns a user prompt such as:

> "Explain how RAG systems work for production teams"

into a polished technical article with:

- topic classification and routing
- optional real-time web evidence retrieval
- article structure planning
- section-by-section drafting
- markdown output with citations and references where applicable
- optional AI image generation for diagrams and explanatory visuals
- live execution status in the browser through streaming updates

## Core capabilities

- Smart routing: closed-book, hybrid, and open-book modes based on topic needs
- Research-aware planning: Tavily-powered sourcing for current or topic-sensitive content
- Structured article generation: goals, subpoints, target words, and section metadata
- Parallel writing workers: independent drafting of each article section
- Reducer pipeline: merges sections into a polished final blog
- Visual planning: decides whether technical diagrams are helpful
- Streaming interface: updates are pushed in real time to the frontend
- Persistent workflows: LangGraph checkpointer stores execution state in PostgreSQL
- Local-first deployment: Ollama provides a self-hosted LLM option

## System architecture

The application is built as a LangGraph state machine with a clear execution flow:

1. Router determines whether the topic requires fresh web research.
2. Research node gathers evidence from Tavily when needed.
3. Orchestrator creates a structured blog plan.
4. Worker nodes write each section independently.
5. Reducer merges all sections into one article.
6. Image planner decides whether diagrams are beneficial.
7. Optional AI-generated images are inserted into the markdown output.
8. Final content is saved and served for download.

## Project structure

```text
Agentwriter-AI/
├── app.py
├── backend.py
├── requirements.txt
├── README.md
├── LICENSE
├── templates/
│   └── index.html
├── static/
│   ├── css/
│   └── jss/
├── outputs/
├── images/
├── notebooks/
│   ├── 1.basic_blog_agent.ipynb
│   ├── 2.blog_agent_with_updated_prompt.ipynb
│   ├── 3.blog_agent_with_research.ipynb
│   ├── 4_blog_agent_with_image_gen.ipynb
│   └── ...
├── demo.excalidraw
└── .env.example (if added later)
```

## Technology stack

- Python 3.11+
- FastAPI for API delivery and backend endpoints
- LangGraph for workflow orchestration
- LangChain for model abstractions and tool integration
- Ollama for local model execution
- Tavily Search for external evidence retrieval
- PostgreSQL for LangGraph checkpointing
- Jinja2 for frontend templating
- Optional: Groq and Google Gemini integrations

## Prerequisites

Before running the agent, ensure the following are available:

- Python 3.11+
- pip package manager
- PostgreSQL database for checkpoint persistence
- Ollama installed locally
- Tavily API key for web research
- Optional: Groq API key and/or Google API key for alternate providers or image generation

### Local model setup

Pull the default model used by the workflow:

```bash
ollama pull llama3.1:8b
```

The project expects the local Ollama endpoint at:

```text
http://localhost:11434
```

## Environment configuration

Create a `.env` file in the root directory with the required values:

```env
DATABASE_URL=postgresql://username:password@host:port/database?sslmode=require
TAVILY_API_KEY=your_tavily_api_key
GROQ_API_KEY=your_groq_key_if_used
GOOGLE_API_KEY=your_google_api_key_if_used
```

Notes:

- `DATABASE_URL` is required because the app initializes a Postgres-backed LangGraph checkpointer.
- If the URL lacks `sslmode=`, the app appends it automatically.
- The app expects a local Ollama instance and a persistent database backend.

## Installation

```bash
git clone <repository-url>
cd Agentwriter-AI
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Run locally

Start the application:

```bash
python app.py
```

or:

```bash
uvicorn app:app --host 127.0.0.1 --port 8000 --reload
```

Then open the app in your browser:

```text
http://127.0.0.1:8000
```

## API surface

### Health check

```http
GET /api/health
```

### Execute a workflow run

```http
POST /api/run
```

Request body:

```json
{
  "topic": "How modern RAG systems are changing developer tooling"
}
```

The response is a streaming SSE payload with live updates for:

- routing decision
- research discovery
- article planning
- section completion
- image planning
- final markdown assembly

### Download generated article

```http
GET /api/runs/{run_id}/download
```

This returns the final markdown file generated by the workflow.

## Execution flow

```text
User Topic
  -> Router
  -> Research (optional)
  -> Orchestrator / Planner
  -> Section Workers
  -> Reducer
  -> Visual Planner
  -> Final Markdown Output
```

## Use cases

This project is useful for:

- AI-assisted technical writing
- automated internal knowledge publishing
- research-driven content generation
- rapid blog drafting and editorial prototyping
- product demos for AI writing agents

## Production considerations

For production deployment, consider adding:

- secret management for API keys and database credentials
- TLS and reverse proxy configuration
- input validation and rate limiting
- logging and monitoring for workflow execution
- persistent storage for generated outputs and run metadata
- user authentication if exposed to multiple teams or clients

## License

This project is distributed under the license located in the repository root.

## Contributing

Contributions are welcome. Recommended future enhancements include:

- better prompt tuning for article quality and consistency
- support for more LLM providers and model routing
- richer editorial review and revision workflows
- stronger source citation and fact-checking pipelines
- export options beyond markdown

## Summary

Agentwriter-AI is a research-aware, workflow-driven AI writing system designed for technical content generation. It combines generation, retrieval, and orchestration into a practical architecture for creating high-quality blog content with minimal manual effort.
