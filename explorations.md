# GitReplay Explorations

This document captures open design directions, future opportunities, and questions worth exploring before or during implementation.

## 1. Product Positioning

GitReplay can be framed in several ways:

### A. Repo replay tool
Focus on time-based commit replay and project evolution visualization.

### B. Repo documentary engine
Focus on narrative explanation, decision analysis, and educational playback.

### C. Software evolution research platform
Focus on structured data for mining software history, architecture drift, and developer behavior.

### D. Onboarding copilot for codebases
Focus on helping new developers understand how the current design emerged.

The strongest long-term positioning may combine B and D.

## 2. Replay Units

### Commit-level replay
Pros:
- fully faithful to Git history
- simple implementation

Cons:
- noisy
- hard to follow for active repos

### Episode-level replay
Pros:
- better for teaching
- aligns with feature/fix/refactor arcs

Cons:
- requires clustering heuristics or model help

### Milestone replay
Pros:
- concise
- executive-friendly

Cons:
- loses detail

Likely recommendation:
- support all three
- make episode mode the default educational view

## 3. Intent Inference

One of the hardest problems is inferring why code changed.

Possible evidence sources:
- commit messages
- PR titles / descriptions
- issue references
- tests added or changed
- config and dependency changes
- uploaded architecture docs
- external references to stack patterns

Exploration question:
How reliable can intent inference be when commit messages are weak?

Possible answer:
Use confidence scoring and present multiple plausible interpretations when needed.

## 4. Repo Identity and Sharing

Using canonical GitHub URL as the stable repo ID is a clean design decision.

Interesting future directions:
- shared public analysis pages for well-known repos
- versioned replay snapshots by release
- compare forks with parent repos
- cross-repo lineage analysis

## 5. Popularity-Aware Persistence

Current rule:
- persist analysis for repos above configurable star threshold
- initial threshold = 100 stars

Exploration ideas:
- combine stars with forks and request frequency
- auto-promote frequently analyzed repos into durable cache
- let users pin low-star repos
- prioritize popular repos for background refresh

## 6. LLM Provider Modes

### Claude Code mode
Potential strengths:
- strong local workflow experience
- natural fit for code exploration
- tool-using reasoning patterns

Questions:
- CLI wrapper vs MCP integration?
- how should structured output validation work?

### OpenRouter default mode
Potential strengths:
- easy onboarding
- cheap or free entry point

Questions:
- how to handle changing free-model availability?
- how to expose fallback behavior clearly?

### User API mode
Potential strengths:
- flexibility
- enterprise adoption path

Questions:
- should the backend proxy provider calls or let clients call directly?

## 7. Uploaded Documents as Grounding

This is likely high value for real projects.

Useful document types:
- ADRs
- specs
- architecture docs
- meeting notes
- migration plans
- research notes

Key questions:
- should documents be attached per repo or reusable across repos?
- how should chunking preserve section hierarchy?
- how should citations appear in explanations?
- should diagram OCR or figure extraction be supported later?

## 8. Reusable Knowledge Memory

This is one of the strongest differentiators.

Instead of sending raw docs repeatedly, GitReplay can distill and reuse:
- concept cards
- pattern cards
- stack-specific summaries
- common tradeoff notes
- onboarding sequences

Interesting exploration:
Can the platform learn reusable explanations for technology patterns without overfitting to one repo?

Examples:
- caching often appears after performance pain
- tests often spike after regressions or stabilization
- architecture boundaries often harden before external API growth

## 9. Web Learning Mode

This should be optional and carefully constrained.

Good source types:
- official docs
- papers
- quality books or book metadata
- reputable tutorials

Questions:
- should web learning happen only when retrieval confidence is low?
- should users be able to whitelist or blacklist sources?
- how should citations be surfaced in replay mode?

## 10. Knowledge Storage Model

A simple RAG system may be too shallow for this project.

More interesting model:

### Layer 1: Raw source storage
- repo text
- docs
- external materials

### Layer 2: retrieval index
- vectors
- metadata
- lexical search

### Layer 3: distilled concept graph
- technologies
- concepts
- patterns
- relationships

### Layer 4: teaching assets
- concept cards
- pattern cards
- guided onboarding paths

Exploration question:
How much value does graph-based knowledge add compared with simple hybrid retrieval?

## 11. Frontend Design Options

### D3-heavy design
Pros:
- strong custom timeline and dense visualization support

Cons:
- more custom engineering effort

### React Flow-heavy design
Pros:
- easier interactive graph exploration

Cons:
- less ideal for dense temporal storytelling

Likely split:
- D3 for timeline, churn, and playback animations
- React Flow for architecture and dependency exploration

## 12. Export Formats

Future export ideas:
- shareable public replay page
- markdown explanation report
- narrated slide deck
- short video export
- onboarding bundle for a new engineer

## 13. Research Potential

GitReplay could also become a research platform for:
- software evolution mining
- code archaeology
- architectural drift analysis
- commit-to-design alignment
- AI-assisted explanation quality evaluation

Possible publication directions:
- software engineering workshops
- mining software repositories venues
- AI for software engineering tracks

## 14. Evaluation Ideas

Potential evaluation questions:
- do users understand a repo faster with replay vs static documentation?
- does uploaded-doc grounding improve explanation trust?
- how much latency/cost reduction comes from reusable knowledge storage?
- how well do episode explanations align with PR descriptions or human author feedback?

## 15. Possible Future Features

- compare two branches as parallel development stories
- replay only one subsystem or directory
- generate “watch these 8 moments first” onboarding path
- explain architectural drift over time
- detect recurring bug or churn hotspots
- support repo-to-repo learning transfer
- generate documentation from evolution history instead of only current state

## 16. Near-Term Decisions to Make

1. Choose initial backend language and framework.
2. Choose parser strategy for MVP.
3. Decide whether the first UI is a web player or API-first.
4. Decide whether Claude Code support starts as CLI mode or later MCP mode.
5. Define minimal schema for episodes and explanations.
6. Define privacy defaults for uploaded docs and external LLM use.
7. Decide what should be generated eagerly vs on demand.

## 17. Recommended First Experiments

### Experiment 1
Run replay on a small clean public repo and measure whether episode clustering feels meaningful.

### Experiment 2
Compare explanation quality in these modes:
- repo only
- repo + uploaded docs
- repo + stored concept cards
- repo + docs + web enrichment

### Experiment 3
Test cache savings on a popular public repo above the star threshold.

### Experiment 4
Measure whether a reusable knowledge store improves explanation quality and cost for multiple repos using the same tech stack.

