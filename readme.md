# LLM Reasoning on Financial Data with Knowledge Graphs + RAG

Experiments in building a **Neo4j knowledge graph from SEC EDGAR Form 10-K filings** and querying it with vector search and RAG. Everything runs in Jupyter notebooks — no package, no CLI.

## Notebooks

| Notebook | What it does |
| --- | --- |
| `text_embeddings.ipynb` | Vector index + similarity search on Neo4j's sample *Movies* graph — the warm-up. |
| `data_processing.ipynb` | Chunks a 10-K from `data/` and loads the chunks into Neo4j as `:Chunk` nodes. |
| `vector_embeddings.ipynb` | Embeds the chunks, then answers questions over them via Cypher search and a LangChain retriever. |

Run `data_processing.ipynb` → `vector_embeddings.ipynb` for the financial pipeline.

## Setup

Requires Python 3.10+, a Neo4j 5+ instance with the GenAI plugin, and an OpenAI-compatible API key.

```bash
python -m pip install -r requirements.txt
```

Copy `.env.example` to `.env` and fill in `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`, `NEO4J_DATABASE`, `OPENAI_API_KEY`, `OPENAI_BASE_URL`. `.env` is gitignored — don't commit credentials.

## Notes

- Embeddings are computed **inside Neo4j** via `genai.vector.encode`, not in Python.
- Chunks are keyed by `chunkId` with a uniqueness constraint, so re-running the load is safe.
- Sample data is NetApp's FY2023 10-K.

Patterns follow DeepLearning.AI's *Knowledge Graphs for RAG* course, rebuilt on current `langchain-neo4j` packages.
