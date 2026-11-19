## Service Endpoints

### Internal Docker Network: ai-network

| Service | Endpoint | Purpose |
|---|---|---|
| Ollama | http://ollama:11434 | LLM inference |
| Open WebUI | http://open-webui:8080 | AI chat interface |
| PostgreSQL | postgres:5432 | Relational database |

### Host Endpoints

| Service | Endpoint | Purpose |
|---|---|---|
| NGINX | http://localhost:8080 | Web server |
| Ollama | http://localhost:11434 | LLM inference |
| Open WebUI | http://localhost:3000 | AI chat interface |
| PostgreSQL | localhost:5432 | Relational database |

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

### Infrastructure Configuration

- Root directory: `/opt/ai`
- Compose files: `/opt/ai/services`
- Persistent data: `/opt/ai/data`
- Shared Docker network: `ai-network`
- Credentials: Local `.env` files excluded from Git
- GPU allocation: Ollama only