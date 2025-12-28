# 🧠 AI Chat Agent Backend: Multi-Agent Orchestration System
---
A state-of-the-art **Agentic AI Backend** engineered with **FastAPI** and **LangGraph**. This system moves beyond RAG, deploying a **supervised multi-agent architecture** capable of handling complex workflows, maintaining long-term memory, and executing real-world actions via a suite of integrated tools.

Designed for scalability, it features **human-in-the-loop (HITL)** controls, **real-time streaming**, and **multimodal** processing.

![Thumnail](https://github.com/abdullahal-baki/AI-Chat-Agent-Doc/blob/main/assets/thumnail.png)

## 🚀 System Architecture & Capabilities

### 1. Supervised Multi-Agent Workflow
At the core is a **Routing Supervisor**, an intelligent node that analyzes user intent and dynamically routes tasks to specialized sub-agents:

- **🤖 Chatbot Agent:** Handles general conversational context and RAG pipelines.

- **🎬 Movie Manager:** A specialized SQL-backed agent for querying catalogs, fetching metadata (IMDb), and managing media databases.

- **📧 Email Manager:** Authenticated Gmail integration to read threads and draft/send emails.

- **💻 Code Agent:** A developer-focused agent capable of generating, debugging, and explaining complex code snippets.

### 2. Human-in-the-Loop (HITL) & Safety
The system implements a robust checkpointer mechanism (SQLite) to manage state.

- **Interruption:** Sensitive actions (like sending an email or executing SQL) trigger an automatic pause.

- **Resumption:** The API supports `Command(resume=...)`, allowing the user to approve, reject, or modify the agent's proposed action before execution.

### 3. Advanced Memory Systems
We utilize a hybrid memory architecture to simulate human-like recall:

- **Session State:** SQLite Checkpointers preserve the exact state of the graph, allowing conversations to pause and resume across server restarts.

- **Long-Term Memory:** Integrated **Mem0** local database to store user preferences and facts permanently.

- **Vector Knowledge:** Pinecone and FAISS vector stores for semantic retrieval of documents and headlines.

### 4. High-Fidelity Streaming & Multimodal
- **Real-Time SSE:** Implements Server-Sent Events (SSE) to deliver token-by-token streaming responses (`/live-stream`), mimicking the UX of ChatGPT/Claude.

- **Multimodal Inputs:** Native parsing support for **PDF, DOCX, CSV, Excel, and Images,** allowing agents to analyze file content seamlessly.

- **Voice Pipeline:** End-to-end voice support using **Deepgram** for transcription and helper utilities for Text-to-Speech (TTS).


---------------

## 🛠️ Technical Stack
| Domain | Technologies Used |
|---|---|
| **Core Framework** | Python 3.10+, FastAPI, Pydantic v2, NextJS |
| **AI Orchestration** | **LangGraph**, LangChain, LangSmith |
| **LLM Inference** | **Groq** / OpenAI / Google GenAI (pluggable)|
| **Data Persistence** | **SQLAlchemy**, SQLite (Checkpoints), **Mem0** (User Memory) |
| **Vector Search** | **Pinecone**, FAISS, Sentence Transformers |
| **Web & Scraping** | **Crawl4AI**, Tavily, Wikipedia API, BeautifulSoup |
| **Integrations** | Gmail API, OpenWeatherMap, IMDb |
| **DevOps** | Docker, Alembic (Migrations), GitHub Actions, AWS |

----------------------

## 🔌 API Gateway Highlights
The backend exposes a RESTful API designed for modern frontend consumption (Next.js):

#### 💬 Conversational Endpoints
- `POST /api/v1/chat/live-stream` — **SSE Streaming.** The primary endpoint for real-time chat with streaming tokens.
- `POST /api/v1/text` — **Standard Chat.** Returns full JSON responses, including tool calls and HITL interrupt signals.
- `POST /api/v1/voice` — **Audio Pipeline.** Accepts audio → Transcribes → Processes → Returns Audio/Text.
- `POST /api/v1/code` — **Dev Mode.** Direct access to the coding agent for technical queries.

#### 🧠 State & Memory Management
History & State
- `GET /api/v1/get-thread-ids` – List active threads
- `GET /api/v1/chat_history/{thread_id}` – Retrieve chat history
- `DELETE /api/v1/delete_thread/{thread_id}` – Delete a thread


Memory (Mem0)
- `POST /api/v1/add_memory`
- `GET /api/v1/get_all_memory`
- `GET /api/v1/get_single_memory/{memory_id}`
- `PUT /api/v1/update_memory`
- `PUT /api/v1/delete_memory/{memory_id}`

#### ⚙️ Misc
- `GET /api/v1/updates` – Entry page content updates

![API Routers](https://github.com/abdullahal-baki/AI-Chat-Agent-Doc/blob/main/assets/api_routers.png)

----------------------------

## 📂 Project Structure
```
(Short Overview)

app/
├── ai/
│   ├── graph/          # LangGraph Workflow Definitions (Supervisor, Multimodal)
│   ├── nodes/          # Executable Units (Agents, Tools, Middlewares)
│   ├── tools/          # Tool Implementations (Gmail, Context7, Weather, News)
│   ├── prompts/        # System Prompts & Persona Definitions
│   └── states/         # Graph State Schemas (TypedDict/Pydantic)
├── db/                 # Database Layer (FAISS Indices, SQLite Files)
├── routers/            # FastAPI Route Handlers (Chat, History, Memory)
├── models/             # API Request/Response Schemas
├── utils/              # Helpers (Transcriber, Scraper, Notifications)
└── config/             # Environment & Configuration Loaders

```
```
(Full Project Tree)
.
├─ alembic/
│  ├─ env.py
│  ├─ README
│  └─ script.py.mako
├─ app/
│  ├─ ai/
│  │  ├─ LLM/
│  │  │  ├─ get_llm.py
│  │  │  └─ __init__.py
│  │  ├─ embeddings/
│  │  │  ├─ get_embeddings.py
│  │  │  └─ __init__.py
│  │  ├─ graph/
│  │  │  ├─ chatbot_graph.py
│  │  │  ├─ code_agent_graph.py
│  │  │  ├─ multimodal_agent_graph.py
│  │  │  └─ __init__.py
│  │  ├─ nodes/
│  │  │  ├─ get_agents.py
│  │  │  ├─ get_tools.py
│  │  │  ├─ middlewares.py
│  │  │  ├─ response.py
│  │  │  └─ __init__.py
│  │  ├─ prompts/
│  │  │  ├─ system_prompts.py
│  │  │  └─ __init__.py
│  │  ├─ states/
│  │  │  ├─ state.py
│  │  │  └─ __init__.py
│  │  ├─ tools/
│  │  │  ├─ context7_tools.py
│  │  │  ├─ gmail_tools.py
│  │  │  ├─ langchain_tools.py
│  │  │  ├─ mcp_tools.py
│  │  │  ├─ movie_catalog_tools.py
│  │  │  ├─ news_tools.py
│  │  │  ├─ vector_retrievers.py
│  │  │  ├─ weather_tools.py
│  │  │  └─ web_tools.py
│  │  ├─ utils/
│  │  |  ├─ memory_management.py
│  │  |  └─ __init__.py
│  │  ├─ chatbot.py
│  │  ├─ __init__.py
│  ├─ db/
|  |  └─faiss
|  |  |  ├─memory.faiss
|  |  |  └─memory.pkl
│  |  ├─ ai_headlines.pkl
│  |  ├─ bdnews_headlines.pkl
│  |  ├─ dev_headlines.pkl
│  |  ├─ jago_headlines.pkl
│  |  ├─ chatbot_agent_memory.db
│  |  └─ movie_database.db
│  ├─ config/
│  │  ├─ configer.py
│  │  └─ __init__.py
│  ├─ models/
│  │  ├─ request.py
│  │  ├─ response.py
│  │  └─ __init__.py
│  ├─ routers/
│  │  ├─ chat.py
│  │  ├─ history.py
│  │  ├─ memory_router.py
│  │  ├─ miscellaneous.py
│  │  └─ __init__.py
│  ├─ utils/
│  │  ├─ news_scraper.py
│  │  ├─ notifier_bot.py
│  │  ├─ transcriber.py
│  │  └─ __init__.py
│  ├─ app.py
│  ├─ __init__.py
├─ notebooks/
│  ├─ long-term-memory-test.ipynb
│  ├─ movie-management-agent.ipynb
│  └─ MultiModal.ipynb
├─ tests/
│  ├─ conftest.py
│  ├─ test_tools.py
│  └─ test_tools_utils.py
├─ .env
├─ .gitignore
├─ README.md
├─ requirements.txt
├─ requirements-dev.txt
├─ pyproject.toml
├─ setup.cfg
└─ Dockerfile
```

---

# 🖼️ Screenshots

## 💬 Chat Interface
![Dashboard](https://github.com/abdullahal-baki/AI-Chat-Agent-Doc/blob/main/assets/entry_page.png)

## 👤 Account Access
![Login](https://github.com/abdullahal-baki/AI-Chat-Agent-Doc/blob/main/assets/login.png)

---

# 🧪 Monitoring (LangSmith)

The system includes full observability:
- Agent traces  
- Prompt debugging  
- Error tracking  
- RAG performance visualization  

---

# 🏁 Final Result

A complete, autonomous **AI Agent** ecosystem capable of:
- **Advanced LLM Reasoning:** Harnesses the full strength of state-of-the-art models (OpenAI, Gemini) for complex decision-making.
- **Hyper-Personalized Memory:** Retains user context and preferences (Short-Term & Long-Term) to deliver highly tailored responses.
- **Inbox Automation:** Autonomously manages email workflows (Read, Draft, Send) with optional human oversight.
- **Real-Time Intelligence:** Fetches and synthesizes up-to-the-minute data using advanced web search tools.
- **Universal Integration:** Designed with an extensible architecture to connect seamlessly with any external API or digital service.

---

# 📬 Contact / Credits

If you want to extend or customize this system:  
**Developer:** _Md Abdullah Al Baki_  
**Email:** _abdullahalbaki009@gmail.com_  

---






























