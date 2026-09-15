# Sovereign Autopackager (`sovereign-autopackager`)

[![PyPI Version](https://img.shields.io/badge/pypi-v1.0.0-blue.svg)](pyproject.toml)
[![Tests](https://img.shields.io/badge/pytest-passing_100%25-brightgreen.svg)](tests/test_solution.py)
[![CI](https://github.com/BlackFoxgamingstudio/autopackager/actions/workflows/ci.yml/badge.svg)](https://github.com/BlackFoxgamingstudio/autopackager/actions/workflows/ci.yml)
[![n8n Integration](https://img.shields.io/badge/n8n-workflow_ready-orange.svg)](n8n/workflow.json)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Enterprise Standalone Package**: Meta-developer tooling that automatically wraps raw source code into standardized Python libraries with pyproject.toml, pytest suites, Dockerfiles, and n8n webhook adapters, plus blameless post-mortem templates.

---


## 🚦 Architecture & Implementation Status

| Component | Status | Details |
|---|---|---|
| **Architecture Tier** | **TIER 2 (INFRASTRUCTURE LIVE / LOGIC QUEUED)** | **Tier 2: Foundation & Integration Live**. Docker containerization, GitHub Actions CI/CD, OpenAPI 3.1 REST API, Zero-Trust `X-SBB-Auth` webhook adapter, and n8n canvas nodes are fully production-ready. Domain algorithms are documented in `docs/ARCHITECTURE.md` and tracked on the `ROADMAP.md` backlog. |
| **REST Gateway** | ✅ Live on Port `8797` | `POST /api/v1/execute`, `GET /health` |
| **Security Layer** | ✅ Active | Enforced `X-SBB-Auth` token verification |
| **n8n Orchestration** | ✅ 100% Connected | Full 3-node connected execution pipeline |
| **Domain Logic** | ⏳ Scaffolded (v1.1.0 Backlog) | Tracked in [`ROADMAP.md`](ROADMAP.md) |
| **Automated Tests** | ✅ Passing | `python3 -m unittest discover -s tests` |
## 1. Overview & Architectural Blueprint

`sovereign-autopackager` is an independently packaged, zero-dependency software library and microservice engineered as part of Russell Alan Powers' 10-year software engineering portfolio.

It delivers robust capabilities in **Meta-Engineering & DevOps Tooling** and provides seamless integration with n8n event workflows.

```
┌───────────────────────────┐         HTTP POST          ┌───────────────────────────────────────────┐
│        n8n Engine         │ ─────────────────────────> │        sovereign-autopackager Adapter        │
│   (Port 5678 Webhook)     │ <───────────────────────── │             (Port 8797)                 │
└───────────────────────────┘       Idempotent JSON      └───────────────────────────────────────────┘
                                                                               │
                                                                               ▼
                                                         ┌───────────────────────────────────────────┐
                                                         │            CoreEngine Domain              │
                                                         │      (SHA-256 Idempotent Execution)       │
                                                         └───────────────────────────────────────────┘
```

---

## 2. Core Exported Classes & Features

- **Primary Module**: `from sovereign_autopackager import LibraryScaffolder, TestGenerator, WebhookWrapperSynthesizer, EngineeringDebriefTemplate`
- **Deterministic Idempotency**: All executions generate unique SHA-256 idempotency tokens preventing duplicate runs across network retries.
- **Self-Contained Microservice**: Zero external third-party dependencies required for base execution.

---

## 3. Installation & Quickstart

```bash
# Clone the repository
git clone https://github.com/russellpowers/sovereign-autopackager.git
cd autopackager

# Install in editable mode
pip install -e .

# Verify health status via CLI
sovereign-autopackager --health
```

---

## 4. CLI Usage Reference

```bash
# Check service health
sovereign-autopackager --health

# Execute core domain action with a JSON payload
sovereign-autopackager --exec process_data --payload '{"sample_key": "sample_value"}'
```

---

## 5. n8n Automation & Integration Contract

- **Microservice Port**: `http://localhost:8797`
- **Inbound Trigger Route**: `POST /api/v1/execute`
- **Integration Workflow**: `New code repo committed -> Auto-scaffold Python package -> Generate pytest suite -> Register in master database`

### How to Import into n8n:
1. Open your n8n canvas (`http://localhost:5678`).
2. Click **Workflows** > **Import from File**.
3. Select `n8n/workflow.json`.
4. Start the background webhook adapter:
   ```bash
   python3 n8n/webhook_adapter.py
   ```
5. Dispatch your test event to `http://localhost:5678/webhook/autopackager-trigger`.

---

## 6. Verification & Automated Testing

This repository includes a 100% passing test suite runnable via `pytest` or `python3`:

```bash
# Run tests with pytest
pytest tests/test_solution.py -v

# Run tests directly (zero dependencies)
python3 tests/test_solution.py
```

---

## 7. Staff/Principal Engineer Technical Defense

> **60-Second Interview Pitch**:
> "Proves Staff/Principal level thinking: creating tools that build tools, enforcing engineering standards, and accelerating developer velocity."
