# NovaCart RAG Metrics Intelligence Agent

Automated, grounded Retrieval-Augmented Generation (RAG) system built on n8n, Pinecone, and OpenAI for enterprise metrics intelligence.

---

## Overview

The **NovaCart RAG Metrics Intelligence Agent** is an automated operations tool designed to ingest multi-format operational data (documents, PDFs, spreadsheets) from Google Drive, index them semantically into a vector database, and serve strict, citation-backed metrics answers to leadership.

Rather than relying on generic, ungrounded LLM summaries that risk hallucinating financial and operational numbers, this agent strictly enforces internal vector search retrieval and semantic reranking. It provides immediate executive insights regarding weekly metrics, product catalog specs, and annual operational health without manual analyst intervention.

---
## Architecture Overview

Data Ingestion Pipeline (Offline/Batch Processing): Scheduled job that extracts enterprise documents, chunks them, generates vector embeddings, and indexes them into Pinecone. 

The agent ingests internal enterprise documents across multiple formats to form its core knowledge vault:

* **Annual Reports (Context & Strategy):** `NovaCart Company Annual Report 2024.docx`

* **Product Catalog (Hardware & Services):** `NovaCart_Product_Catalog.pdf`

* **Sales & Metrics (Conversion Data):** `SKU_Weekly_Sales_Conversion_3Y_with_Revenue.docx`

+-----------------------------------------------------------------------------------+
|                            1. DATA INGESTION PIPELINE                             |
|                                                                                   |
| [Schedule Trigger] ---> [Search Google Drive] ---> [Download Files]              |
|                                                            |                      |
|                                                            v                      |
| [Pinecone Index] <--- [OpenAI Embeddings] <--- [Text Splitter] <--- [Data Loader] |
+-----------------------------------------------------------------------------------+

```text
[ Schedule Trigger ]
         │
         ▼
[ Search files and folders ] ──> (Google Drive)
         │
         ▼
[ Download file ] ──────────────> (Google Drive)
         │
         ▼
[ Pinecone Vector Store ] ─────────────────────────────────────────┐
    ├── (Document)   ──> [ Default Data Loader ]                  │
    │                        └── (Text Splitter) ──> [ Recursive Character Text Splitter ]
    │                                                             │
    └── (Embeddings) ──> [ Embeddings OpenAI ] ───────────────────┘

```

---

## Tech Stack

* **Logic & Orchestration:** n8n


* **Intelligence & Agent Brain:** OpenAI (`gpt-4.1-mini`, `text-embedding-3-large`)


* **Vector Database:** Pinecone Vector Store


---

## Prerequisites & Credentials

Ensure you have active accounts and API keys for the following services:


| Service | Purpose | Required Credentials / Scope 

| **n8n** | Automation Workflow Engine | Self-hosted or Cloud Instance 

| **Google Cloud** | Document Ingestion | OAuth 2.0 Client ID & Secret with Drive API enabled

| **Pinecone** | Vector Database | API Key, Serverless Index (Dimensions: 1024, Metric: Cosine)

| **OpenAI** | LLM & Embeddings | API Key (`gpt-4.1-mini`, `text-embedding-3-large`)


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

* **RAG-Ingestion_V1.json:** The full export file containing the scheduled Google Drive to Pinecone indexing workflow.

---

## Repository Contents

* **`README.md`:** Project documentation, data flow diagrams, and architectural overview.


* **`RAG-Ingestion_V1.json`:** Importable n8n workflow JSON for batch document processing.
  

* **`RAG Ingestion workflow.pdf`:** Contains high-resolution screenshots of the workflow configuration.


* **`Internal Resources`:** Contains files for internal RAG ingestion pipeline


