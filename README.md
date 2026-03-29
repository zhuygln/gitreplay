# gitstori.com

gitstori.com helps you understand how open-source projects were built by turning their commit history into interactive lessons. It uses AI models to analyze how a codebase evolved over time and generates web pages that walk you through the development story.

This is not a tool for learning Git or GitHub. It is a tool for learning **codebases** — how they were designed, how they grew, and how they work.

## URL Swap

gitstori.com mirrors GitHub's URL structure. Swap `github.com` for `gitstori.com` in any repository URL to get its lesson page:

```
github.com/owner/repo  →  gitstori.com/owner/repo
```

Projects can link directly to their lesson page, and it just works.

## How It Works

A model reads through a repository's commit history, branches, and changes, then produces an HTML page that illustrates how the project evolved — what was built, in what order, and why.

## Two Ways to Use It

### Local Tool

Download and run gitstori locally with your own model (e.g., Claude Code). Point it at any repository on your machine and it generates an HTML page you can open in your browser. You own the process, use your own tokens, and everything stays local.

### Hosted Website (gitstori.com)

Visit gitstori.com, submit a GitHub URL, and get back a generated lesson page on the fly. No setup required.

#### Additional Website Features

**Daily updates on selected GitHub users**
Curated profiles of selected GitHub users are processed locally using the site operator's own API and uploaded to the database. Visitors can browse these pre-generated results without triggering a model call, keeping costs predictable and access instant.

**Project proposals and sponsorships**
Processing a large codebase with its full history can require significant token usage. Users can propose or sponsor the cost of processing a specific repository. This lets the community fund lessons for important projects that would be too expensive for on-the-fly generation.

## Status

Early planning stage.
