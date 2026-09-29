# Stock Intelligence Platform — Specification

**Live site:**

- English: https://prachaya-orr.github.io/stock-intelligence-spec/
- ภาษาไทย (Thai): https://prachaya-orr.github.io/stock-intelligence-spec/th/

This repo hosts the spec-driven development document for the Stock Intelligence Platform, published as static HTML pages in English and Thai. The spec covers the product, the domain and the software architecture. Each page has a link to switch to the other language.

## What the platform does

The Stock Intelligence Platform keeps collecting company information from primary sources, mainly SET, SEC and company investor-relations pages. It turns that information into verifiable structured facts and recalculates financial metrics from them. When new information may change an assumption in an investment thesis, it sends an alert backed by evidence.

Every alert is meant to answer:

1. What changed?
2. Which source proves it?
3. Which number or assumption is affected?
4. Why might it matter?
5. How confident is the system?
6. What remains unverified?

## Architecture at a glance

| Area | Choice |
|---|---|
| Backend | Go microservices split by bounded context |
| Frontend | Next.js + TypeScript |
| Service internals | Pragmatic Ports & Adapters |
| Messaging | Kafka-compatible events, Transactional Outbox, idempotent consumers |
| Storage | PostgreSQL (+ pgvector), S3-compatible object storage, Redis |
| Contracts | OpenAPI for HTTP, AsyncAPI + JSON Schema for events |
| Primary market | Thailand / SET-listed securities |

Core rules:

- **Evidence before narrative:** every material claim links back to its exact source location.
- **Deterministic numbers:** financial calculations run in versioned code, never in an LLM.
- **Labelled claims:** each claim is marked as fact, calculation, inference or hypothesis.
- **Data ownership:** each service writes only to its own database.

## What the document covers

- Product vision, scope and non-goals
- Microservice boundaries, event catalogue and data ownership
- Core domain model, financial calculation rules and restatement handling
- AI analyst rules: what it may and may not do, plus its output contract
- Go service layout, coding rules and repository structure
- HTTP API surface, security, reliability targets and observability
- Testing strategy, deployment model and CI/CD
- Spec-driven workflow, requirement and ADR templates, initial requirements
- Delivery roadmap, accepted ADRs, open questions and definition of done

## Repository contents

| File | Purpose |
|---|---|
| `index.html` | English HTML rendering of the spec, served by GitHub Pages |
| `th/index.html` | Thai HTML rendering of the spec |
| `.nojekyll` | Tells GitHub Pages to serve files as-is without Jekyll processing |

Diagrams are rendered in the browser with [Mermaid](https://mermaid.js.org/), which is loaded from a CDN. The Thai page uses the Noto Sans Thai font from Google Fonts.

## Updating the site

The Markdown sources (`stock-intelligence-spec-driven-development.md` and `stock-intelligence-spec-driven-development-th.md`) are kept outside this repo. After regenerating the HTML from them:

```bash
cp ../stock-intelligence-spec-driven-development.html index.html
cp ../stock-intelligence-spec-driven-development-th.html th/index.html
git add -A
git commit -m "Update spec"
git push
```

GitHub Pages redeploys from `main` automatically, usually within a minute.

## Status

Version **0.1.0** (English) and **0.1.0-th** (Thai) are the baseline specification, dated 2026-09-30.
