<div align="center">

# AI CRM Agent

### Production Claude agents that read and write CRM data safely. MCP tools, three-layer memory, confirmation-gated writes, immutable audit log.

![License](https://img.shields.io/badge/license-MIT-3b82f6)
![Built by Hyperion AI](https://img.shields.io/badge/built_by-Hyperion_AI-0f172a)
![Stack](https://img.shields.io/badge/stack-Claude_·_MCP_·_Next.js-64748b)
![GitHub Repo stars](https://img.shields.io/github/stars/Hyperion-AI-Agency/ai-crm-agent?style=flat&color=fbbf24)
![Last Commit](https://img.shields.io/github/last-commit/Hyperion-AI-Agency/ai-crm-agent?color=3b82f6)

---

<img src="docs/architecture.svg" width="100%" alt="Architecture" />

[Design Doc](DESIGN_DOC.pdf) · [Architecture](docs/architecture.md) · [Quickstart](#quick-start) · [Book a call](https://cal.com/vitalijus-alsauskas/project-request?overlayCalendar=true)

</div>

> [!NOTE]
> This is a reference architecture shared as a starting point, not a production service. Fork it, swap the CRM adapter for your stack, ship faster.

## What this is

A full-stack Claude agent for teams extending chatbots into CRM territory. Exposes CRM reads and writes as MCP tools, composes three memory layers (session + user + account) at prompt time, and gates every write behind explicit user confirmation.

Claude + CRM chatbots usually fail in two predictable ways: they lose memory across sessions (users re-explain context constantly) or they write to CRM without proper review (surprise records on Monday morning). This agent is built around the pattern that avoids both.

## Why this exists

- **MCP tools** - expose CRM capabilities via Model Context Protocol. Swap CRMs without touching the agent.
- **Three memory layers** - session (current conversation), user (preferences, recent interactions), account (company-wide context). Composed at prompt time.
- **Confirmation-gated writes** - every mutating tool goes through a guardrail. Agent drafts, user approves, commit fires. Nothing silently changes.
- **Immutable audit log** - every tool call + commit logged to append-only S3 with before/after. Regulatory-grade.
- **Isolation per user** - one user cannot see another's memory or audit trail.

## Documentation

**[Design Doc (PDF)](DESIGN_DOC.pdf)** — Branded system design with diagrams, tool choices, phases, and risks.

**[Architecture](docs/architecture.md)** — Layered view with component responsibilities and design decisions.

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
make dev
```

Runs install + docker + migrations + dev server in one command. See `make help` for the full list.

> [!TIP]
> Prefer not to run this yourself? [Book a 30-minute scoping call](https://cal.com/vitalijus-alsauskas/project-request?overlayCalendar=true) and I'll map this to your CRM and agent setup.

## Who this is for

**Firms extending an existing Claude setup** into CRM-aware chatbot capabilities (not greenfield - if you're starting from scratch, the foundations are here too).

**Developers hired to build a Claude+CRM chatbot** - fork this repo, the MCP + memory + guardrail pattern is the hardest part and it's already figured out.

**Technical reviewers** - skim the architecture and flow diagrams to see what production-ready Claude + CRM looks like.

---

<table>
<tr>
<td width="240" valign="top">
<img src="https://hyperionai.dev/founder.png" width="220" alt="Vitalijus Alsauskas" />
</td>
<td valign="top">

### Vitalijus Alsauskas

Software and AI Engineer

Software dev from Lithuania, worked at IBM, living in Vietnam. Building enterprise-level tools for startups and companies.

[LinkedIn](https://www.linkedin.com/in/vitalijus-hyperion/) · [GitHub](https://github.com/Vitals9367) · [hyperionai.dev](https://hyperionai.dev)

</td>
</tr>
</table>

---

### Request a project

Send me your project brief. I reply within 24 hours.

**[cal.com/vitalijus-alsauskas/project-request →](https://cal.com/vitalijus-alsauskas/project-request?overlayCalendar=true)**

Or reach out via [LinkedIn](https://www.linkedin.com/in/vitalijus-hyperion/).

---

## License

MIT. Fork, adapt, ship.
