## Service Endpoints

### Internal Docker Network: ai-network

| Service | Endpoint | Purpose |
|---|---|---|
| Ollama | http://ollama:11434 | LLM inference |
| Open WebUI | http://open-webui:8080 | AI chat interface |
| PostgreSQL | postgres:5432 | Relational database |
| Qdrant (REST) | http://qdrant:6333 | Vector storage and similarity search |
| Qdrant (gRPC) | qdrant:6334 | gRPC vector database API |

### Host Endpoints

| Service | Endpoint | Purpose |
|---|---|---|
| NGINX | http://localhost:8080 | Web server |
| Ollama | http://localhost:11434 | LLM inference |
| Open WebUI | http://localhost:3000 | AI chat interface |
| PostgreSQL | localhost:5432 | Relational database |
| Qdrant (REST) | http://localhost:6333 | Vector database API |
| Qdrant (gRPC) | localhost:6334 | gRPC vector database API |
| Qdrant Dashboard | http://localhost:6333/dashboard | Vector database administration |

### Ollama API Endpoints

| Endpoint | Method | Purpose |
|---|---|---|
| /api/tags | GET | List installed models |
| /api/ps | GET | List loaded models |
| /api/generate | POST | Generate completion |
| /api/chat | POST | Chat completion |
| /api/embed | POST | Generate embeddings |
| /api/pull | POST | Download model |
| /api/delete | DELETE | Delete model |
| /api/show | POST | Model information |
| /v1/chat/completions | POST | OpenAI-compatible chat |
| /v1/embeddings | POST | OpenAI-compatible embeddings |

### Persistent Storage

| Service | Host Directory |
|---|---|
| NGINX | /opt/ai/data/nginx |
| Ollama | /opt/ai/data/ollama |
| Open WebUI | /opt/ai/data/open-webui |
| PostgreSQL | /opt/ai/data/postgres |
| Qdrant | /opt/ai/data/qdrant |

### Infrastructure Configuration

- Root directory: `/opt/ai`
- Compose files: `/opt/ai/services`
- Persistent data: `/opt/ai/data`
- Shared Docker network: `ai-network`
- Credentials: Local `.env` files excluded from Git
- GPU allocation: Ollama only

### Qdrant API Endpoints

Base URL: `http://qdrant:6333`

| Endpoint | Method | Purpose |
|---|---|---|
| / | GET | Service and version information |
| /healthz | GET | Health check |
| /collections | GET | List collections |
| /collections/{name} | GET | Collection information |
| /collections/{name} | PUT | Create collection |
| /collections/{name} | DELETE | Delete collection |
| /collections/{name}/points | PUT | Insert or update vectors |
| /collections/{name}/points/query | POST | Vector similarity search |
| /collections/{name}/points/scroll | POST | Browse stored points |
| /collections/{name}/points/count | POST | Count stored points |
| /collections/{name}/points/delete | POST | Delete points |

### Qdrant Configuration

- Storage engine: Qdrant
- GPU allocation: None
- Persistent storage: `/opt/ai/data/qdrant`
- Docker network: `ai-network`
- Collection dimensions: Determined by embedding model
- Distance metric: Configured per collection
- Authentication: Disabled by default
- External access: Enabled