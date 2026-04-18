<div align="center">

# AI CRM Agent

### Production Claude agents that read and write CRM data safely. MCP tools, three-layer memory, confirmation-gated writes, immutable audit log.

![License](https://img.shields.io/badge/license-MIT-3b82f6)
![Built by HyperionAI](https://img.shields.io/badge/built_by-HyperionAI-0f172a)
![Stack](https://img.shields.io/badge/stack-Claude_·_MCP_·_Next.js-64748b)

[View Design Doc (PDF)](DESIGN_DOC.pdf) · [Architecture](docs/architecture.md) · [Implementation Plan](IMPLEMENTATION_PLAN.md)

---

![Architecture](docs/architecture.svg)

</div>

## What this is

A production pattern for teams extending Claude into CRM-aware chatbot capabilities. Demonstrates how to expose CRM reads and writes as MCP tools, layer memory across session + user + account, and gate every write behind user confirmation.

Claude + CRM chatbots usually fail in two predictable ways: they lose memory across sessions (users re-explain context constantly) or they write to CRM without proper review (surprise records). This reference shows the pattern that works.

## Why this exists

- **MCP tools** - expose CRM capabilities via Model Context Protocol. Swap CRMs without touching the agent.
- **Three memory layers** - session (current conversation), user (preferences, recent interactions), account (company-wide context). Composed at prompt time.
- **Confirmation-gated writes** - every mutating tool goes through a guardrail. Agent drafts, user approves, commit fires. Nothing silently changes.
- **Immutable audit log** - every tool call + commit logged to append-only S3 with before/after. Regulatory-grade.
- **Isolation per user** - one user cannot see another's memory or audit trail.

## Documentation

**[Design Doc (PDF)](DESIGN_DOC.pdf)** — Branded system design with diagrams, tool choices, phases, and risks.

**[Architecture](docs/architecture.md)** — Layered view with component responsibilities and design decisions.

**[Implementation Plan](IMPLEMENTATION_PLAN.md)** — 3-phase build plan with milestones and risks.

**[Flow Diagram](docs/flow.svg)** — Sequence diagram of the confirmation-gated write path.

## Stack

| Layer | Technology |
|-------|-----------|
| Chat UI | Next.js 15 (App Router, streaming) |
| Agent Runtime | FastAPI + Anthropic SDK |
| Tool Layer | Model Context Protocol (MCP) server in Python |
| Memory | Postgres (session + user + account) + pgvector |
| Audit Log | Immutable S3 (Object Lock) + Postgres mirror |
| CRM | HubSpot / Salesforce / Pipedrive (adapter per backend) |
| LLM | Anthropic Claude 3.5 Sonnet (zero-retention) |

## Quick Start

```bash
pnpm install
docker compose -f docker-compose.local.yml up -d
cd apps/api && poetry install && poetry run alembic upgrade head && cd ../..
pnpm dev
```

## Who this is for

**Firms extending an existing Claude setup** into CRM-aware chatbot capabilities (not greenfield - if you're starting from scratch, the foundations are here too).

**Developers hired to build a Claude+CRM chatbot** - fork this repo, the MCP + memory + guardrail pattern is the hardest part and it's already figured out.

**Technical reviewers** - skim the architecture and flow diagrams to see what production-ready Claude + CRM looks like.

---

<div align="center">

## About the author

<img src="https://hyperionai.dev/founder.png" width="120" style="border-radius:50%" alt="Vitalijus Alsauskas" />

### Vitalijus Alsauskas

Founder, HyperionAI

[LinkedIn](https://www.linkedin.com/in/vitalijus-hyperion/) · [GitHub](https://github.com/Vitals9367) · [hyperionai.dev](https://hyperionai.dev)

</div>

4 years at IBM on Fortune 500 systems, including AskProcurement (chatbot integrating Dun & Bradstreet data for procurement teams - same pattern as what's in this repo). Claude Code and MCP are my daily setup. Currently running Claude in production on Adboard (Next.js + Supabase + Claude API with OAuth into Meta, Google, Shopify) and Fit7D (FastAPI + Claude dialogue platform).

---

<div align="center">

## Interested in building something like this?

I offer a free 30-minute scoping call for teams extending Claude into CRM territory.

### [Book a call](https://cal.com/vitalijus-alsauskas/project-request?overlayCalendar=true)

or reach out via [LinkedIn](https://www.linkedin.com/in/vitalijus-hyperion/)

</div>

---

## License

MIT. Fork, adapt, ship.
