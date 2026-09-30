# Production RAG Assistant

A clean, production-style Retrieval-Augmented Generation backend built with **Python** and **FastAPI**.

The project is intentionally dependency-light so it can run immediately without a paid LLM or vector database. The retrieval layer can later be replaced with OpenAI embeddings, pgvector, Qdrant, Pinecone, Weaviate, or another production stack.

## Features

- FastAPI REST API
- Document ingestion
- Automatic text chunking
- Vector-style similarity retrieval
- Ranked context retrieval
- Source citations
- Health endpoint
- Docker support
- Unit/API tests
- Environment configuration
- Clean service-oriented structure

## Architecture

```text
Client
  |
  v
FastAPI
  |
  +--> Document Ingestion
  |       |
  |       v
  |    Chunking
  |       |
  |       v
  |    In-memory Index
  |
  +--> Query
          |
          v
       Retrieval
          |
          v
      Top-K Context
          |
          v
   Answer + Citations
```

## Project Structure

```text
production-rag-assistant/
├── app/
│   ├── __init__.py
│   ├── main.py
│   └── rag.py
├── tests/
│   └── test_api.py
├── .env.example
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── Makefile
├── requirements.txt
└── README.md
```

## Run Locally

```bash
python -m venv .venv
```

Activate the virtual environment, then:

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Open:

```text
http://127.0.0.1:8000/docs
```

## API Usage

### Add a document

```bash
curl -X POST "http://127.0.0.1:8000/documents"   -H "Content-Type: application/json"   -d '{
    "text": "RAG combines retrieval with language generation.",
    "source": "rag-notes"
  }'
```

### Query the knowledge base

```bash
curl -X POST "http://127.0.0.1:8000/query"   -H "Content-Type: application/json"   -d '{
    "question": "What does RAG combine?",
    "top_k": 3
  }'
```

Example response:

```json
{
  "question": "What does RAG combine?",
  "answer": "Based on the indexed context...",
  "citations": [
    {
      "source": "rag-notes",
      "chunk_id": "document-id:0",
      "score": 0.61
    }
  ]
}
```

## Run Tests

```bash
pytest -q
```

## Docker

```bash
cp .env.example .env
docker compose up --build
```

## Production Upgrade Path

For a real production deployment, replace the demo retrieval layer with:

- OpenAI / Voyage / Cohere embeddings
- PostgreSQL + pgvector or Qdrant
- Hybrid BM25 + vector search
- Cross-encoder reranking
- LLM answer generation
- Streaming responses
- Authentication and rate limiting
- Observability and evaluation pipeline

## Resume / Portfolio Description

> Built a production-style Retrieval-Augmented Generation service using FastAPI with document ingestion, semantic-style retrieval, ranked context selection, source citations, Docker support, and automated tests.

## License

MIT
