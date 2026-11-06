# 📄 Building a Document Q&A Agent with RAG (n8n + Groq + Ollama)

An end-to-end Retrieval-Augmented Generation (RAG) conversational agent built in **n8n**. This workflow allows users to upload documents (such as HR Policy PDFs, company guidelines, or custom knowledge bases), generate vector embeddings locally via **Ollama**, store vector representations, and query the knowledge base using an **AI Agent** powered by **Groq Chat Model**.

---

## 📌 Features

- **Document Ingestion Pipeline:** Form trigger enabling easy uploading of document files (PDFs, text files, markdown, etc.).
- **Smart Chunking:** Employs `Recursive Character Text Splitter` to chunk large documents effectively while preserving context boundaries.
- **Local Embeddings (Ollama):** Vector embeddings generated locally using Ollama (`n8n-nodes-langchain.embeddingsOllama`), ensuring speed and security.
- **Vector Store Indexing:** Inserts split chunks into a vector store to build an queryable knowledge index (`Insert Data to Store` / `knowledge_base`).
- **Conversational RAG AI Agent:** An interactive AI Agent equipped with memory and vector retrieval tools to answer questions directly based on ingested context.
- **Groq Integration:** Uses high-speed LLM inference via **Groq Chat Model** for near-instant response generation.

---

## 🏗️ Workflow Architecture Breakdown

```
========================= INGESTION PIPELINE =========================
[Upload your file here] (Form Trigger)
       │
       ▼
[Insert Data to Store] ◄─── (Text Splitter) ◄─── [Recursive Character Text Splitter]
       │               ◄─── (Document Loader) ◄─ [Default Data Loader]
       │               ◄─── (Embeddings) ─────── [Embeddings Ollama]
       ▼
[Vector Store Index]

========================= QUERY / RAG PIPELINE =========================
[When chat message received] (Chat Trigger)
       │
       ▼
   [AI Agent]
   ├── Chat Model ────► [Groq Chat Model]
   └── Tool ──────────► [knowledge_base (Vector Store)] ◄── [Embeddings Ollama]
```

### Node Sequence Explained

1. **Ingestion Trigger (`Upload your file here`):** Web form trigger for uploading target documents (e.g., HR Policy Document).
2. **Text Processing (`Default Data Loader` & `Recursive Character Text Splitter`):** Parses raw document contents and splits text into optimal chunk sizes.
3. **Embedding Generation (`Embeddings Ollama`):** Converts text chunks into dense vector representations using Ollama.
4. **Vector Indexing (`Insert Data to Store`):** Stores embeddings and text metadata into the RAG vector engine.
5. **Chat Interface (`When chat message received`):** Listens for user queries in the n8n chat UI.
6. **Agent & Retrieval (`AI Agent` + `knowledge_base` + `Groq Chat Model`):** Queries vector store using embeddings, retrieves top-k relevant document chunks, and synthesizes accurate responses via Groq.

---

## 📋 Example Prompts (HR Policy Testing)

Once your HR policy or knowledge base document is uploaded, test the chat agent with these queries:

1. *What is the process for applying leave?*
2. *What happens if I exceed my leave balance?*
3. *What is the notice period for employees?*
4. *How does the company handle sick leave?*
5. *Can an employee carry forward unused leaves?*
6. *What is the policy for working during probation?*
7. *What are the rules for termination?*
8. *What benefits are provided to full-time employees?*

---

## ⚙️ Prerequisites & Setup

### 1. Ollama (Local Embeddings)
Ensure Ollama is running on your machine with an embedding model (e.g., `nomic-embed-text` or `all-minilm`):

```bash
# Pull an embedding model in terminal
ollama pull nomic-embed-text
```

### 2. Groq API Key
Obtain an API key from [Groq Console](https://console.groq.com/) and configure it under **Groq Chat Model** node credentials in n8n.

### 3. Workflow Import & Execution
1. In n8n, click **Menu (...)** $\rightarrow$ **Import from File**.
2. Upload this workflow JSON file.
3. Execute the document upload pipeline first by submitting a file through **Upload your file here**.
4. Open the **Chat UI** (`Open chat` button at bottom canvas) and start asking questions!

---

## 📄 License
Distributed under the MIT License.