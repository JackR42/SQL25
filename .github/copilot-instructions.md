# SQL25 Copilot Instructions

## Project Purpose
SQL25 is a proof-of-concept (POC) for **vector embeddings and vector search** using SQL Server 2025. The project demonstrates semantic search capabilities using vector data types and indexing features native to SQL Server 2025.

## Technology Stack
- **SQL Server 2025**: Running in devcontainer with vector search capabilities
- **Vector Embeddings**: Generate embeddings for semantic search (consider: OpenAI, Hugging Face, or local models)
- **Vector Indexing**: Leverage SQL Server 2025's built-in vector index types (HNSW, IVF)
- **Runtime**: Python or .NET (client code for embedding generation and search queries)

## Architecture & Key Patterns

### Repository Structure (Expected)
```
SQL25/
├── .devcontainer/          # SQL Server 2025 container config
├── scripts/
│   ├── schema/             # Vector table definitions, indexes
│   └── seed/               # Sample data with embeddings
├── src/
│   ├── embeddings/         # Embedding generation logic
│   └── search/             # Vector search query utilities
├── tests/                  # Integration tests with SQL Server
└── README.md               # Project overview & setup
```

### Database Patterns
- **Vector columns**: Use `vector` type (dimension varies by embedding model)
- **Indexing strategy**: HNSW for fast approximate search, IVF for large-scale data
- **Connection string**: Use dev container hostname (typically `sqlserver` or `localhost,1433`)
- **Auth**: Environment variables for SA_PASSWORD (dev only)

### Development Workflow
1. Start devcontainer: Loads SQL Server 2025 automatically
2. Initialize schema: Create vector tables, seed with test embeddings
3. Test embedding pipeline: Generate embeddings locally, insert into SQL Server
4. Implement search: Query vector index with k-nearest neighbor search
5. Iterate: Profile performance, optimize indexes/models

### Critical Files to Know
- `.devcontainer/docker-compose.yml` or `Dockerfile`: SQL Server 2025 setup
- Database initialization scripts: Schema creation and vector index definitions
- Embedding generation module: Where embeddings are created before insertion
- Search query utilities: How to execute vector search (KNN, distance metrics)

## Common Pitfalls & Solutions
- **Dimension mismatch**: Embedding dimension must match vector column definition
- **Performance**: Vector indexes require proper sizing and distance metrics (cosine, L2, dot product)
- **Dev container startup**: First SQL Server start can take 30-60 seconds; wait for readiness
- **Seed data**: Always include representative test embeddings for search validation

## When Adding Features
- Update `.devcontainer` config if dependencies change
- Add schema changes to migration scripts (version-controlled)
- Document embedding model specifics (dimensions, normalization)
- Include vector search performance benchmarks in tests
- Keep this instruction file synchronized with discovered patterns
