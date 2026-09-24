# NovaCart RAG Metrics Intelligence Agent

Automated, grounded Retrieval-Augmented Generation (RAG) system built on n8n, Pinecone, and OpenAI for enterprise metrics intelligence.

---

## Overview

The **NovaCart RAG Metrics Intelligence Agent** is an automated operations tool designed to ingest multi-format operational data (documents, PDFs, spreadsheets) from Google Drive, index them semantically into a vector database, and serve strict, citation-backed metrics answers to leadership.

Rather than relying on generic, ungrounded LLM summaries that risk hallucinating financial and operational numbers, this agent strictly enforces internal vector search retrieval and semantic reranking. It provides immediate executive insights regarding weekly metrics, product catalog specs, and annual operational health without manual analyst intervention.

---

## Tech Stack

* **Logic & Orchestration:** n8n


* **Intelligence & Agent Brain:** OpenAI (`gpt-4.1-mini`, `text-embedding-3-large`)


* **Vector Database:** Pinecone Vector Store


* **Reranking Engine:** Cohere (`rerank-v3.5`)


* **Interface & Styling:** n8n Hosted Chat Widget (with Custom Glass-morphism CSS)



---

## Data Architecture

The agent ingests internal enterprise documents across multiple formats to form its core knowledge vault:

* **Annual Reports (Context & Strategy):** `NovaCart Company Annual Report 2024.docx`

* **Product Catalog (Hardware & Services):** `NovaCart_Product_Catalog.pdf`

* **Sales & Metrics (Conversion Data):** `SKU_Weekly_Sales_Conversion_3Y_with_Revenue.docx`


---

## Core Logic & Triage Rules

Every user query submitted through the chat interface is evaluated via strict agent execution rules:

1. **Mandatory Vector Search:** For all business metrics or operational queries, the agent MUST query the `Nova Pinecone Vector Index` tool exactly once using a derived 5–12 word search term before attempting an answer.


2. **Strict Anti-Hallucination Guardrail:** If the retrieved vector chunks are empty or do not contain the requested metric/timeframe, the agent is strictly forbidden from inventing data and MUST reply with the exact string:


> *"I’m sorry, I don’t have that information in my knowledge base. Please try asking a different question."*
> 


3. **Small-Talk Exception:** Standard greetings (e.g., "hi", "hello") bypass the vector tool and trigger a polite conversational response.


4. **Hybrid Web Search Fallback:** If a user query explicitly asks for competitive analysis or market comparisons against external vendors, the agent utilizes its built-in Web Search tool to supplement internal findings.



---

## Execution Workflow

The system operates across two distinct asynchronous pipelines:

### Offline Data Ingestion Batch

1. **Schedule Trigger:** Triggers batch ingestion on a periodic cadence (e.g., daily at midnight).


2. **Drive Retrieval:** Queries Google Drive for target metric files and downloads binary payloads.


3. **Chunking:** Data is loaded via `Default Data Loader` and split using `Recursive Character Text Splitter` (Chunk Size: 500, Overlap: 50).


4. **Embedding & Vectorization:** Text chunks are transformed into 1024-dimensional vectors using OpenAI's `text-embedding-3-large` model and stored under the `Production` namespace in Pinecone.



---

## Workflow & Architecture Configuration

### n8n Blueprints

* **Data Ingestion.json:** The full export file containing the scheduled Google Drive to Pinecone indexing workflow.

---

## Repository Contents

* **`README.md`:** Project documentation, data flow diagrams, and architectural overview.


* **`System Prompt.txt`:** System instructions controlling agent decision-making, vector execution enforcement, and anti-hallucination guardrails.


* **`Custom Chat Styling.css`:** Custom CSS snippet providing dark glass-morphism UI theme for the n8n chat widget.


* **`Data Ingestion.json`:** Importable n8n workflow JSON for batch document processing.
  

* **`Data Ingestion workflow.pdf`:** Contains high-resolution screenshots of the workflow configuration.
