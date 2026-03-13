# GitReplay Plan

## Goal
Build a repository-learning platform that replays how a GitHub project evolved over time and explains the development story using deterministic repository facts plus optional AI assistance.

## Product Scope

### In scope
- GitHub repo import by URL
- canonical GitHub URL as repo identity
- commit and episode replay
- semantic code change analysis
- optional LLM explanations
- optional uploaded document support
- optional web learning mode
- reusable knowledge backend for similar future repos
- popularity-aware caching based on configurable star threshold

### Out of scope for first MVP
- private enterprise SSO integrations
- full video rendering pipeline
- broad support for every VCS host
- full offline local LLM runtime
- heavy IDE plugin integrations

## Non-Goals
- replacing GitHub UI
- replacing full documentation systems
- claiming perfect reconstruction of developer intent
- fully autonomous web research without source controls

## Product Requirements

### Functional requirements
1. Accept a GitHub URL and normalize it into a canonical repo ID.
2. Clone or fetch repository history.
3. Extract commit graph, branches, tags, and diffs.
4. Group commits into higher-level episodes.
5. Provide a timeline replay UI.
6. Analyze changed files, symbols, tests, and dependencies.
7. Route explanation requests to one of several LLM providers.
8. Optionally ingest PDFs and text documents.
9. Optionally enrich explanations with external materials.
10. Persist analysis for popular repos above configurable star threshold.
11. Reuse prior stored analysis when repository head has not changed.
12. Incrementally refresh persisted analysis when upstream changes occur.

### Non-functional requirements
- deterministic repo facts must remain available independent of LLM output
- explanations should expose evidence sources and confidence
- support cost-aware caching and deduplication
- allow privacy-sensitive configurations
- provide fast incremental re-analysis for cached popular repos

## Architecture Overview

### Core services

#### 1. Repo Ingestion Service
Responsibilities:
- normalize GitHub URLs
- clone/fetch repo
- read commit DAG
- collect metadata such as stars, default branch, head SHA

Outputs:
- canonical repo metadata
- commit graph
- file change records

#### 2. Snapshot / Diff Service
Responsibilities:
- reconstruct repo state over time
- create sparse checkpoints
- generate per-commit or per-episode diffs

#### 3. Semantic Analysis Service
Responsibilities:
- parse code structure
- detect symbol-level changes
- classify change intent
- infer architecture and subsystem shifts

#### 4. Episode Builder
Responsibilities:
- cluster commits into meaningful development arcs
- score salience of events
- produce replay units for UI and explanation engine

#### 5. Knowledge Ingestion Service
Responsibilities:
- ingest uploaded PDFs and text files
- parse and chunk supporting project docs
- store metadata, chunks, embeddings, and citations

#### 6. Reusable Knowledge Store
Responsibilities:
- store reusable technical materials
- maintain concept cards and pattern cards
- support cross-project retrieval for similar stacks

#### 7. Retrieval Orchestrator
Responsibilities:
- combine repo facts, uploaded docs, reusable knowledge, and optional web materials
- build evidence packs for explanations

#### 8. LLM Provider Router
Responsibilities:
- choose between Claude Code workflow, OpenRouter default lane, or user-provided API
- route based on settings, cost, and availability

#### 9. Explanation Service
Responsibilities:
- produce structured episode explanations
- generate onboarding narratives
- answer repo questions grounded in evidence packs

#### 10. API / UI Layer
Responsibilities:
- expose replay endpoints
- support episode browsing and search
- support user settings and context-source toggles

## Data Model

### Repository
- repo_id (canonical GitHub URL)
- host
- owner
- repo_name
- stars
- forks
- default_branch
- last_seen_head_sha
- persist_policy

### Analysis
- analysis_id
- repo_id
- analysis_version
- scope_type
- scope_ref
- head_sha
- status
- created_at
- updated_at

### Commit / File Change / Symbol Change
- commit metadata
- parent relationships
- changed files
- diff stats
- symbol deltas

### Episode
- episode_id
- repo_id
- title
- category
- salience score
- linked commits
- linked subsystems

### Knowledge Objects
- uploaded_document
- document_chunk
- web_material
- concept_card
- pattern_card
- explanation_cache

## Provider Strategy

### Supported modes
1. Claude Code integration mode
2. OpenRouter default hosted mode
3. user-supplied API / endpoint mode

### Routing policy
- use explicit user choice first
- otherwise use default hosted lane
- support fallback when the free/default lane is unavailable
- use cheaper models for tagging and classification tasks
- reserve stronger models for deeper explanation mode

## Cache and Persistence Policy

### Repository identity
Use canonical GitHub URL as stable repo identity.

### Persisted analysis rule
If repo stars exceed `star_cache_threshold`, persist analysis artifacts.

Initial config:
- `star_cache_threshold = 100`

### Reuse logic
- if stored head SHA equals current head SHA, reuse
- if head SHA changed, run incremental update
- if analysis schema or parser version changed, mark stale and rebuild as needed

### Suggested policies
- popular public repos: durable shared cache
- low-star public repos: short-lived cache unless pinned
- private/user repos: user-scoped persistence options

## Phased Delivery

## Phase 0: Repo foundation
- create repo structure
- add starter docs
- define configuration model
- choose license and contribution setup

Deliverables:
- README
- planning docs
- initial config schema

## Phase 1: Deterministic replay MVP
- GitHub URL ingestion
- commit graph extraction
- basic diff replay
- timeline API
- initial episode grouping heuristics

Deliverables:
- backend import pipeline
- simple timeline UI or JSON replay endpoint

## Phase 2: Semantic analysis
- parser integration
- symbol extraction
- file/category classification
- architecture delta summaries

Deliverables:
- semantic event pipeline
- richer episode model

## Phase 3: LLM layer
- provider router
- structured explanation outputs
- evidence pack builder
- source typing in UI/API

Deliverables:
- explanation endpoint
- provider configuration support

## Phase 4: Uploaded docs + retrieval
- PDF/text ingestion
- chunking and indexing
- repo-doc linking
- retrieval orchestration

Deliverables:
- project context upload feature
- grounded explanations using docs

## Phase 5: Reusable knowledge memory
- concept-card distillation
- cross-repo tech fingerprint matching
- retrieval reuse for similar stacks

Deliverables:
- knowledge reuse backend
- lower-cost repeated explanations

## Phase 6: Web learning mode
- curated source search
- source ranking and caching
- optional external learning mode

Deliverables:
- external enrichment pipeline
- web-backed teaching mode

## Open Questions
- how much episode building should be rules vs model-assisted?
- should explanations be generated eagerly or lazily?
- what privacy guarantees are needed for uploaded docs?
- when should low-star repos still be persisted?
- how should UI expose confidence and evidence types?

## Risks
- explanation quality may vary with weak commit messages
- parsing quality may vary by language
- cost can grow without aggressive caching
- web enrichment can reduce trust if source ranking is weak
- legal boundaries matter for storing books or copyrighted material

## Success Metrics
- time to first replay
- explanation latency
- percentage of reused cached analysis
- percentage of explanation tokens saved via reusable knowledge
- user-rated helpfulness for onboarding and understanding repo history

