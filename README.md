# Chroma Context-1 Data Generation

Synthetic data generation pipeline from our [technical report](https://www.trychroma.com/research/context-1).

Generates synthetic multi-hop search tasks across multiple domains. Each domain follows an explore → verify → extend pattern to produce multi-step retrieval tasks.

Context-1 model weights are available [here](https://huggingface.co/chromadb/context-1).

## Setup

```bash
# Install dependencies
uv sync

# For all optional dependencies (reranking, patents, indexing)
uv sync --all-extras

# Configure environment
cp .env.example .env  # then fill in API keys
```

### Required environment variables

| Variable | Used by |
|----------|---------|
| `ANTHROPIC_API_KEY` | All domains |
| `SERPER_API_KEY` | Web (search + scrape) |
| `JINA_API_KEY` | Web (backup page fetcher) |
| `OPENAI_API_KEY` | Web, SEC, Email, Patents (embeddings) |
| `CHROMA_API_KEY` | Web, SEC, Email, Patents (indexing) |
| `CHROMA_DATABASE` | Web, SEC, Email, Patents (indexing) |
| `BASETEN_API_KEY` | SEC (reranking) |

## Domains

Each domain has its own pipeline command and README with full documentation:

- [Web](agentic_search_data_gen/domains/web/README.md) — multi-hop search tasks from the open web
- [SEC](agentic_search_data_gen/domains/sec/README.md) — SEC filing tasks
- [Patents](agentic_search_data_gen/domains/patents/README.md) — patent prior-art tasks
- [Email (Epstein)](agentic_search_data_gen/domains/epstein/README.md) — email search tasks

## Project structure

```
agentic_search_data_gen/
├── core/                    # Shared base classes and utilities
│   ├── explore.py           # BaseExplorerAgent
│   ├── extend.py            # BaseExtenderAgent
│   ├── verify.py            # BaseVerifier
│   ├── distract.py          # BaseDistractorAgent
│   ├── rerank.py            # Baseten reranker client
│   ├── indexing.py          # ChromaDB indexing utilities
│   └── utils.py             # Anthropic client, token counting, quote matching
├── domains/
│   ├── web/                 # Web search tasks
│   ├── sec/                 # SEC filing tasks
│   ├── patents/             # Patent prior-art tasks
│   └── epstein/             # Email search tasks
```
