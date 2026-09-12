# Architecture: Sovereign AutoPackager

## Overview

**Package ID:** `PKG-019`  
**Domain:** Meta-Engineering & DevOps Tooling  
**Microservice Port:** `8797`  
**n8n Webhook Path:** `autopackager-trigger`  
**GitHub:** [BlackFoxgamingstudio/autopackager](https://github.com/BlackFoxgamingstudio/autopackager)

Automated solution scaffold generator, packaging engine, and release pipeline manager. Produces new SBB-compatible solution packages with one command.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign AutoPackager        │
                     │       Port: 8797            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  ScaffoldEngine  | ReleaseBuilder  | TemplateLint  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `ScaffoldEngine`
Handles all scaffold operations. Exposes async methods callable from the core dispatcher.

### `ReleaseBuilder`
Handles all release operations. Exposes async methods callable from the core dispatcher.

### `TemplateLinter`
Handles all templatelinter operations. Exposes async methods callable from the core dispatcher.

### `CICDEmitter`
Handles all cicdemitter operations. Exposes async methods callable from the core dispatcher.

### `ManifestValidator`
Handles all manifestvalidator operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-autopackager", "port": 8797}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-autopackager:
  image: sovereign-autopackager:latest
  ports: ["8797:8797"]
  healthcheck:
    test: curl -f http://localhost:8797/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`devops`, `scaffolding`, `automation`, `packaging`
