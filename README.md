# Humanitarian Policy RAG — Recurrent Outbreaks

A retrieval-augmented generation system over humanitarian health policy documents (WHO, OCHA, ICRC), built to surface not just what policy *says* but where it diverges across agencies, lags behind outbreak outcomes, or leaves gaps — the kind of critical read an analyst would bring to a briefing, not just a Q&A bot.

## Why this project

Most RAG portfolio projects retrieve-and-summarize. This one is designed to demonstrate:
- Real-world document ingestion (messy PDFs: tables, multi-column layouts, inconsistent structure)
- Metadata-driven hybrid retrieval (agency, disease, date, document type)
- Comparative / multi-hop retrieval (cross-agency divergence)
- Temporal reasoning (flagging outdated guidance)
- Outcome grounding (policy vs. after-action review contradictions)
- A measurable evaluation harness, not just vibes

## Architecture

```
Documents (WHO / OCHA / ICRC / after-action reviews)
        │
        ▼
  Ingestion pipeline  ──►  data/raw, data/processed
        │
        ▼
  Chunking + Embedding  ──►  Qdrant (vector store, metadata-filtered)
        │
        ▼
  Retrieval  ──►  hybrid search + cross-encoder reranking
        │
        ▼
  Comparative / temporal reasoning layer
        │
        ▼
  Generation (LLM synthesis, analyst-style critical framing)
        │
        ▼
  Evaluation (RAGAS)
```

## Tech stack

| Layer | Tool | Why |
|---|---|---|
| Orchestration | LangChain (+ LlamaIndex for retrieval) | Highest job-market signal; LlamaIndex pairs well for document-heavy retrieval |
| Vector store | Qdrant (self-hosted) | Free, Apache 2.0, scales, metadata filtering |
| Embeddings | sentence-transformers (bge-large) | Free, local, no API dependency |
| Reranker | bge-reranker | Free, open-source cross-encoder |
| Generation | Claude / GPT API | Best quality for nuanced comparative synthesis |
| Evaluation | RAGAS | Open-source, measurable retrieval + answer quality |
| Serving | FastAPI | Lightweight, demonstrable API layer |

## Build order

1. Document ingestion pipeline
2. Chunking + indexing
3. Baseline retrieval (naive RAG loop)
4. Evaluation harness (RAGAS) — before adding complexity
5. Reranking + hybrid search
6. Comparative / multi-hop retrieval
7. Temporal / outcome grounding
8. Critical synthesis layer
9. Interface (FastAPI + minimal frontend or CLI)

## Status

🚧 Early scaffolding — ingestion pipeline in progress.

## Project structure

```
.
├── data/
│   ├── raw/            # untouched source documents
│   └── processed/      # parsed + chunked text with metadata
├── src/
│   ├── ingestion/       # fetch + parse WHO/OCHA/ICRC docs
│   ├── indexing/        # chunking, embedding, vector store loading
│   ├── retrieval/        # hybrid search, reranking, comparative retrieval
│   ├── generation/      # LLM synthesis + critical framing prompts
│   └── evaluation/      # RAGAS evaluation harness
├── notebooks/           # exploration and prototyping
├── tests/
└── configs/             # source lists, prompts, retrieval configs
```
