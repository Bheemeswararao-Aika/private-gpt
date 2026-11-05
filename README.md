# PrivateGPT – Advanced Full-Stack Debugging Challenge

![Banner image](./fern/docs/assets/privategpt_banner.png)

<div align="center">

**PrivateGPT is a production-grade AI API layer and interactive workbench for local and enterprise inference models.**

</div>

---

## 1. Project Overview

PrivateGPT is an enterprise-ready, open-source API layer that connects frontends, agentic workflows, and external applications to local and hosted inference engines (Ollama, llama.cpp, vLLM, OpenAI-compatible APIs). 

The platform provides high-level primitives for production AI systems:
- Multi-turn conversational messages API (`/v1/messages`) mirroring modern AI standards with streaming SSE support.
- Comprehensive document ingestion, parsing, chunking, and vector indexing.
- Agentic retrieval with source attribution and inline citation badges.
- Dynamic tool orchestration including web search, code execution sandboxing, and Model Context Protocol (MCP) servers.
- A built-in reactive Single-Page Application Workbench accessible at `/ui`.

---

## 2. Project Objectives

- Provide an offline-first, privacy-preserving AI orchestration layer.
- Expose a clean, standardized RESTful interface for LLM completions, embeddings, and context-aware chat.
- Enable full observability and deterministic execution across multi-tenant collections.
- Serve as an advanced hands-on engineering environment for full-stack architecture, asynchronous systems, and system-level debugging.

---

## 3. Technology Stack

- **Backend Framework:** FastAPI, Starlette, Uvicorn
- **Language:** Python 3.11+
- **Dependency Injection:** `injector` (Google Guice-style inversion of control)
- **Configuration & Validation:** Pydantic v2, PyYAML, Pydantic-Settings
- **Vector & Storage Engines:** Qdrant, SQLite, PostgreSQL (via SQLAlchemy 2.0 & aiosqlite)
- **Data & Text Processing:** LlamaIndex Core, BeautifulSoup4, Pandas, Libmagic, NLTK
- **Concurrency & Task Workers:** Asyncio, ThreadPoolExecutor, Contextvars, Arq, Celery, Redis
- **Frontend Workbench:** Single-Page Application (HTML5, Modern ES6+ JavaScript, CSS Custom Properties / Dark Glassmorphism, Lucide SVG Icons, Server-Sent Events)
- **API Specification:** OpenAPI 3.1.0 / Fern

---

## 4. Architecture Overview

```text
       +-------------------------------------------------------------+
       |                  PrivateGPT Workbench UI                    |
       |                (Single-Page App at /ui)                     |
       +------------------------------+------------------------------+
                                      | HTTP / SSE
                                      v
       +-------------------------------------------------------------+
       |                   FastAPI Application                       |
       |  Middleware: RequestContextMiddleware, ExceptionMiddleware   |
       +-------+----------------------+-----------------------+------+
               |                      |                       |
               v                      v                       v
       +---------------+      +---------------+       +---------------+
       | Messages &    |      | Ingestion &   |       | Filesystems & |
       | Chat Service  |      | Vector Store  |       | Sandbox Tools |
       +-------+-------+      +-------+-------+       +-------+-------+
               |                      |                       |
               v                      v                       v
       +---------------+      +---------------+       +---------------+
       | LLM Engine    |      | Qdrant /      |       | SQLite /      |
       | (Mock/Ollama) |      | Local Chunks  |       | Migrations DB |
       +---------------+      +---------------+       +---------------+
```

The system employs dependency injection (`private_gpt.di`) to manage singleton service lifecycles across routers, repositories, and hardware abstraction layers.

---

## 5. Prerequisites

Before setting up the project, ensure your environment meets the following requirements:
- **Operating System:** macOS (Apple Silicon / Intel), Linux (Ubuntu 22.04+), or Windows (WSL2 recommended)
- **Python:** Python 3.11.x (>= 3.11, < 3.12)
- **Node.js:** Node 18+ (for UI parsing validation tests)
- **System Libraries:**
  - `libmagic` (macOS: `brew install libmagic` | Ubuntu/Debian: `sudo apt-get install libmagic1`)
- **Git:** Git 2.30+

---

## 6. Installation Instructions

### 1. Clone the Repository
```bash
git clone <repository_url>
cd <repository_name>
```

### 2. Set Up Python Virtual Environment
```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
```

### 3. Install Project Dependencies
```bash
pip install -e ".[test,ingest,tools,queue,redis,observability]"
```

---

## 7. Environment Configuration

PrivateGPT uses layered configuration driven by YAML profiles and environment variables:
- `settings.yaml`: Default system-wide configuration.
- `settings-mock.yaml`: Lightweight mock profile for local testing without external LLM hardware.
- `settings-test.yaml`: Isolation profile for test suites.

To run locally in self-contained mock mode without connecting to an external GPU inference server:
```bash
export PGPT_PROFILES=mock,test
export PGPT_HOME=$(pwd)
```

---

## 8. Database Setup

PrivateGPT automatically runs schema migrations on startup using its built-in migration runner:
- Database files and metadata are stored in `$PGPT_HOME/local_data/`.
- No manual SQL scripts are required; the `PersistenceComponent` applies versioned migrations on application boot.

---

## 9. Frontend & Backend Execution Instructions

### Start the Full-Stack Application
```bash
source .venv/bin/activate
export PGPT_HOME=$(pwd)
export PGPT_PROFILES=mock,test
private-gpt serve --host 0.0.0.0 --port 8080
```

Alternatively, you can launch via python module execution:
```bash
PGPT_HOME=$(pwd) PGPT_PROFILES=mock,test python -m private_gpt.cli.main serve --port 8080
```

### Running Automated Test Suites
```bash
PGPT_HOME=$(pwd) pytest tests/settings/ tests/test_di.py -v
```

---

## 10. Application Access Instructions

Once the server is running, access the following endpoints:
- **Interactive Workbench UI:** [http://localhost:8080/ui/](http://localhost:8080/ui/)
- **Swagger Interactive API Documentation:** [http://localhost:8080/docs](http://localhost:8080/docs)
- **ReDoc API Reference:** [http://localhost:8080/redoc](http://localhost:8080/redoc)
- **Health Check Endpoint:** [http://localhost:8080/health](http://localhost:8080/health)

---

## Advanced Engineering Debugging Challenge

Welcome to the **Advanced Full-Stack Engineering Debugging Challenge**.

This codebase is a realistic, production-grade application that contains multiple hidden, complex defects distributed across various architectural tiers. The application compiles, starts, and passes basic sanity checks, but exhibits subtle failures, performance degradations, data discrepancies, or security gaps under specific operational conditions.

### Your Challenge Objectives

As an advanced engineer, you are required to:
1. **Explore & Analyze:** Understand the end-to-end architecture, dependency graph, and data flow from the browser client to the database and background workers.
2. **Investigate Behavior:** Observe application behavior, inspect console outputs, trace backend logs, analyze network requests, and review test coverage.
3. **Identify Issues:** Uncover hidden functional, technical, concurrency, and security bugs across the system.
4. **Conduct Root Cause Analysis:** Dig deep into the exact technical mechanisms causing each failure. Avoid superficial explanations.
5. **Implement Robust Fixes:** Develop clean, durable, production-ready fixes that preserve original branding, design consistency, and architecture.
6. **Validate & Test:** Write regression tests or clear verification scripts that prove each bug is resolved without introducing regressions.
7. **Document Your Findings:** Prepare a comprehensive technical report documenting your debugging journey, root-cause analyses, code diffs, and verification steps.

---

## 11. Student Expectations

- **Scientific Debugging:** Formulate hypotheses, verify through reproduction, and isolate failing components using logs, debuggers, and test suites.
- **Root-Cause Depth:** Do not apply band-aid fixes (e.g. swallowing exceptions with generic `try/except: pass` or hardcoding magic values). Fix the underlying structural or logical defect.
- **Architectural Respect:** Maintain code style, docstrings, typing annotations, and component boundaries.
- **Verification Proof:** Every claimed resolution must be accompanied by reproducible test steps or automated tests.

---

## 12. Submission Guidelines

1. **Bug Report & Solutions Document:** Submit a structured markdown report containing:
   - Issue identification and classification.
   - Exact steps to reproduce.
   - In-depth Root Cause Analysis (RCA).
   - Code before and after fix (diffs).
   - Validation evidence (test logs or screenshots).
2. **Git Repository:** Submit a clean Git branch with atomic, well-described commits for each resolved issue.
3. **Clean Codebase:** Ensure the application builds cleanly, all existing tests pass, and newly added tests execute reliably.
