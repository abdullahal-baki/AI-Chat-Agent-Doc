# 🧠 AI Chat Agent Backend: Multi-Agent Orchestration System
---
A state-of-the-art **Agentic AI Backend** engineered with **FastAPI** and **LangGraph**. This system moves beyond RAG, deploying a **supervised multi-agent architecture** capable of handling complex workflows, maintaining long-term memory, and executing real-world actions via a suite of integrated tools.

Designed for scalability, it features **human-in-the-loop (HITL)** controls, **real-time streaming**, and **multimodal** processing.

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












