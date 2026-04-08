# Property Valuation RAG System

> Layout-aware retrieval-augmented generation for real estate appraisal documents.

## Overview

A production-ready RAG pipeline that enables accurate Q&A over real estate appraisal PDFs — including appraisals, MLS listings, and assessor records. Built with a custom PDF parser that understands document layout, preserving table structure and valuation context that standard chunkers destroy.

## Problem

- Standard PDF chunkers (recursive text splitters, page-level splits) destroy table structure in appraisal reports
- Comparable sales grids, adjustment tables, and property detail sections get split across chunks, losing critical valuation context
- Generic embeddings miss domain-specific relationships between properties, adjustments, and assessed values

## Architecture

```
Appraisal PDFs (appraisals, MLS, assessor records)
    │
    ▼
Layout-Aware PDF Parser
    │  table + text extraction preserving spatial structure
    ▼
Custom Semantic Chunker
    │  keeps appraisal tables & comp grids intact
    ▼
Embedding + Vector Store
    │  Local:  ChromaDB
    │  Prod:   Vertex AI Vector Search
    ▼
Retrieval + Grounded Generation
    │  Gemini 1.5 Pro with source attribution
    ▼
FastAPI Endpoint
```

## Tech Stack

| Component | Technology |
|---|---|
| Language | Python 3.11+ |
| PDF Parsing | Layout-aware parser (preserves tables and spatial structure) |
| Chunking | Custom semantic chunker |
| Vector Store (local) | ChromaDB |
| Vector Store (production) | Vertex AI Vector Search |
| LLM | Google Gemini 1.5 Pro |
| API | FastAPI |
| Cloud | Google Cloud Platform |

## How to Run

### Prerequisites

- Python 3.11+
- GCP project with Vertex AI enabled (for production mode)
- Gemini API key or GCP service account

### Local Development

```bash
pip install -r requirements.txt
python real_estate_rag_prototype.py
```

### API Endpoint

```bash
uvicorn real_estate_rag_prototype:app --reload
```

The FastAPI server starts at `http://localhost:8000` with interactive docs at `/docs`.

## Production Notes

- **Vector store swap:** ChromaDB is for local dev/prototyping. Switch to Vertex AI Vector Search for production scale and managed infrastructure.
- **Grounded generation:** Gemini 1.5 Pro provides responses with source attribution back to specific document sections.
- **Deployment path:** Cloud Run + Vertex AI Vector Search + Gemini API on GCP.
- **Critical:** Do not replace the custom chunking logic with default recursive text splitters — table-aware chunking is the core differentiator of this system.
- **FastAPI endpoint** supports async and is production-ready for containerized deployment.
