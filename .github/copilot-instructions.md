# SQL25 Copilot Instructions

## Project Purpose
SQL25 is a proof-of-concept (POC) for **vector embeddings and vector search** using SQL Server 2025. The goal is to demonstrate semantic search capabilities using SQL Server's native vector data types and indexing features (HNSW, IVF).

## Current Status (Early Stage)
This project is in early development. Core infrastructure (devcontainer, schema, embedding pipeline) needs to be built out. The project currently contains only:
- Minimal README clarifying project purpose
- Planning phases for devcontainer and vector search implementation
- No implementation code yet

## Key Development Areas to Build

### 1. **Devcontainer Setup**
- Create `.devcontainer/devcontainer.json` and `docker-compose.yml`
- **Expected setup**: SQL Server 2025 container with SA credentials, volume mounts for scripts
- Dev container should expose port 1433 for local SQL Server development
- Reference: Use Microsoft's official SQL Server 2025 container image

### 2. **Database Schema & Scripts** (`scripts/schema/`)
- SQL files defining vector tables with `vector` type columns
- Vector dimensions depend on embedding model (1536 for OpenAI, 768 for BERT, 384 for sentence-transformers)
- Include index definitions:
  - **HNSW**: For <1M vectors when latency is priority
  - **IVF**: For larger datasets when throughput is priority
- Connection string pattern: `Server=localhost,1433;User Id=sa;Password=YourPassword;Encrypt=false`

### 3. **Embedding Pipeline** (`src/embeddings/`)
- Generate embeddings locally (OpenAI API, Hugging Face, or local models)
- **Critical**: Embedding dimension must match vector column definition exactly
- Normalize vectors according to distance metric (cosine normalization, L2, dot product)
- Batch insert embeddings into SQL Server with proper transaction handling

### 4. **Vector Search Implementation** (`src/search/`)
- K-nearest neighbor queries using SQL Server vector search operators
- Support configurable distance metrics and result limits
- Example pattern: `SELECT TOP k ... WHERE column MATCH_VECTOR (embedding) WITH (k=10, metric='cosine')`

### 5. **Testing & Validation** (`tests/`)
- Integration tests verifying embedding → insertion → search pipeline
- Performance benchmarks for different index types and vector counts
- Seed data: `scripts/seed/` contains representative test embeddings for validation

## Development Workflow
1. Initialize devcontainer with SQL Server 2025 and SA password
2. Create vector schema in `scripts/schema/` and seed test data
3. Develop embedding generation logic in `src/embeddings/`
4. Implement vector search queries in `src/search/`
5. Add integration tests and performance benchmarks

## Critical Implementation Notes
- **First SQL Server startup**: Takes 30-60 seconds; wait for container readiness before running scripts
- **Vector dimensions**: Must be consistent between embedding generation and schema definition
- **Embedding normalization**: Different distance metrics require different normalization
- **Batch size tuning**: Balance memory usage against network round-trips for insertion
- **Index creation**: Indexes can be large; monitor disk space for high-dimensional data

## File Structure (Target)
```
SQL25/
├── .devcontainer/
│   ├── devcontainer.json          # Dev container configuration
│   └── docker-compose.yml         # SQL Server 2025 container setup
├── scripts/
│   ├── schema/                    # SQL files: vector tables, indexes
│   └── seed/                      # Sample embeddings for testing
├── src/
│   ├── embeddings/                # Embedding generation & insertion logic
│   └── search/                    # Vector search query utilities
├── tests/                         # Integration test suite
└── README.md
```

## Common Pitfalls to Avoid
- **Dimension mismatch**: Embedding dimension must match vector column definition - will cause insertion failures
- **Index type selection**: HNSW for latency, IVF for throughput; choose before significant data insertion
- **Normalization mismatches**: Ensure embedding normalization matches the distance metric used in queries
- **Memory pressure**: Large vector indexes can consume significant memory; monitor during testing
