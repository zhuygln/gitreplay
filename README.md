# GitReplay

GitReplay turns a GitHub repository into a replayable learning experience.

It reconstructs how a project evolved over time from its Git history, then layers on optional explanations grounded in repository facts, uploaded documents, reusable technical knowledge, and optional web enrichment.

The goal is not just to show **what changed**, but to help users understand:

- how the codebase developed over time
- what engineering decisions likely drove the changes
- how architecture emerged, shifted, or stabilized
- how to onboard faster by learning a repo as a story instead of a static snapshot

## Core Idea

Most repository tools show the current state of a codebase. GitReplay focuses on the **development narrative**.

Instead of only presenting:

`commit -> diff`

GitReplay aims to present:

`problem -> decision -> commit sequence -> architectural consequence`

This makes the platform useful for:

- engineering onboarding
- open-source education
- code archaeology
- architecture reviews
- software evolution research
- AI-assisted repository explanation

## Key Features

### 1. Replay repository history
- Import a GitHub repository by URL
- Reconstruct commit history, branches, tags, and milestones
- Replay development at multiple granularities:
  - commit mode
  - episode mode
  - milestone mode
  - teaching mode

### 2. Semantic change analysis
- Detect file-level changes
- Parse symbols and structure from code
- Identify higher-level changes such as:
  - feature development
  - refactors
  - fixes
  - test hardening
  - dependency or architecture shifts

### 3. Evidence-grounded explanations
GitReplay keeps deterministic repo facts separate from AI-generated interpretation.

AI explanations are built on top of:
- commit metadata
- file and symbol changes
- dependency changes
- tests and docs updates
- optional uploaded documents
- optional reusable technical knowledge
- optional live web materials

### 4. Pluggable LLM providers
GitReplay should support multiple explanation backends:
- local Claude Code workflow support
- default hosted provider through OpenRouter
- user-provided API keys / endpoints

### 5. Optional project document upload
Users may upload supporting material to help explain the repo:
- PDF
- Markdown
- plain text

These documents can include ADRs, design notes, specs, meeting notes, architecture docs, and research notes.

### 6. Optional background learning mode
GitReplay can optionally retrieve external learning material to improve explanations, such as:
- official documentation
- high-quality tutorials
- books or book metadata
- papers and technical references

### 7. Reusable knowledge backend
To reduce latency and token cost, GitReplay stores reusable technical knowledge for later projects using similar stacks and patterns.

Examples:
- FastAPI
- React Flow
- D3
- Tree-sitter
- Redis caching patterns
- MCP / agent tooling

## Repository Identity and Cache Policy

### Canonical repository identity
For GitHub repositories, GitReplay uses the **canonical GitHub URL** as the unique repository identifier.

Canonical form:

`https://github.com/{owner}/{repo}`

Normalization rules:
- lowercase owner and repo name
- remove trailing slash
- remove `.git`
- ignore query params and tree/branch fragments for repo identity

Branch, tag, and commit-range analysis are stored as analysis scope, not as separate repos.

### Popularity-aware persistence
If a repo has more than a configurable number of GitHub stars, GitReplay persists its analysis artifacts for reuse instead of regenerating them each time.

Initial rule:
- `star_cache_threshold = 100`

This threshold should remain configurable.

For persisted repos, GitReplay should:
- reuse cached analysis when the current head SHA matches stored analysis
- perform incremental refresh when new commits appear
- avoid repeating expensive semantic analysis and explanation generation

## System Principles

### Deterministic first
Git history, diffs, symbols, structure, and timeline are the source of truth.

### AI as interpretation layer
LLMs should explain, summarize, compare, and teach, but not replace repository facts.

### Evidence typing
Explanations should clearly distinguish between:
- observed repo facts
- uploaded project docs
- reusable stored knowledge
- live web materials
- model interpretation

### Cost-aware architecture
The system should reuse prior analysis, retrieval artifacts, and distilled technical knowledge whenever possible.

### Optionality
Advanced context sources such as uploaded docs and live web learning should be user-controlled and optional.

## High-Level Architecture

```text
GitHub repo
  -> repo ingestion
  -> semantic change analysis
  -> episode builder
  -> deterministic fact store
  -> retrieval orchestration
  -> LLM provider router
  -> replay + explanation UI
```

Supporting context layers:
- uploaded docs pipeline
- reusable knowledge store
- optional live web learning

## Suggested MVP

### Phase 1
- import repo from GitHub URL
- replay commit history
- show diffs and file tree changes
- basic commit clustering into episodes

### Phase 2
- symbol extraction via parser layer
- classify feature/fix/refactor/test/docs changes
- architecture evolution summaries

### Phase 3
- LLM provider router
- structured episode explanations
- optional uploaded doc support

### Phase 4
- reusable knowledge memory
- optional web enrichment
- onboarding and teaching modes

## Proposed Tech Stack

### Backend
- Python
- FastAPI
- PostgreSQL
- pgvector
- Redis
- background job queue
- Git CLI
- Tree-sitter

### Frontend
- React / Next.js
- D3 for timeline and dense visual analytics
- React Flow for architecture and dependency graph exploration
- Monaco for code / diff viewing

### LLM / retrieval
- provider abstraction layer
- structured outputs
- hybrid retrieval: metadata + full text + embeddings
- concept-card distillation cache

## Initial Repository Docs
This repo starts with:
- `README.md`
- `plan.md`
- `explorations.md`

## Status
Early design / planning stage.

