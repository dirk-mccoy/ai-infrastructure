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

## SearXNG — Web Search Service

SearXNG is a self-hosted, open-source metasearch engine
that aggregates results from multiple upstream search
providers.

It provides centralized web search capabilities for
Open WebUI, AI agents, custom applications, and
conventional browser-based searches.

### Service Configuration

| Parameter | Value |
|---|---|
| Container | searxng |
| Image | searxng/searxng:latest |
| Docker Network | ai-network |
| Host Port | 8088 |
| Container Port | 8080 |
| GPU Required | No |
| External Database Required | No |
| Authentication | None by default |

### Endpoints

| Service | Endpoint |
|---|---|
| Web Interface | http://localhost:8088 |
| Internal Docker API | http://searxng:8080/search |
| Host API | http://localhost:8088/search |
| Preferences | http://localhost:8088/preferences |

### Persistent Storage

Proposed directory structure:

/opt/ai/data/searxng/
├── config/
└── cache/

| Host Directory | Container Directory |
|---|---|
| /opt/ai/data/searxng/config | /etc/searxng |
| /opt/ai/data/searxng/cache | /var/cache/searxng |

SearXNG does not maintain its own web index.
Persistent storage is primarily used for configuration
and cached runtime resources.

### JSON API Configuration

Enable JSON output in settings.yml:

```yaml
search:
  formats:
    - html
    - json
```

Restart the container after modifying configuration:

```bash
docker restart searxng
```

### API Testing

Test search functionality from the host:

```bash
curl -s "http://localhost:8088/search?q=NVIDIA+GB10&format=json" | jq
```

Return only search result titles and URLs:

```bash
curl -s "http://localhost:8088/search?q=NVIDIA+GB10&format=json" \
  | jq '.results[] | {title, url}'
```

Test connectivity from Open WebUI:

```bash
docker exec open-webui curl -s \
  "http://searxng:8080/search?q=NVIDIA+GB10&format=json"
```

### Open WebUI Integration

Navigate to Admin Settings > Web Search.

| Parameter | Value |
|---|---|
| Web Search | Enabled |
| Search Engine | searxng |
| Query URL | http://searxng:8080/search?q=<query> |
| Search Result Count | 5 |
| Concurrent Requests | 3 |

### Browser Integration

SearXNG can also be used as a conventional browser
search engine independently of the AI infrastructure.

Custom browser search URL:

http://<GB10-IP>:8088/search?q=%s

### Operational Commands

```bash
docker logs -f searxng
docker restart searxng
docker stop searxng
docker start searxng
docker stats searxng
```

### Architectural Role

- External information retrieval for LLMs
- Web search API for custom AI agents
- Centralized search provider configuration
- Independent network-wide browser search service
- No commercial API subscription required

### Limitations

- Requires internet connectivity.
- Upstream providers may throttle or block requests.
- Search queries are transmitted to upstream providers.
- Full webpage extraction is a separate operation.
- Service should not be exposed publicly without
  appropriate access controls.