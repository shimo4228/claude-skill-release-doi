# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Planned

- Initial public release.

### What it does

A Claude Code skill that runs the release workflow for DOI-registered research repositories following the identifier-federation triplet from [`authorship-strategy`](https://github.com/shimo4228/authorship-strategy) (ADRs 0001-0003). Sequences five release phases plus one post-release phase, preventing the most common release-time drift incidents.

### Components

- `skills/release-doi/SKILL.md` — the skill body. Five-phase runbook (Baseline, CODEMAPS regeneration, Cross-document consistency, Verify, Release execution) plus one post-release phase (DOI minting wait, concept-DOI verification, citation-metadata follow-up commit).

### Scope

The skill assumes a Git host + tagged-release-triggers-archive-deposit model (the Zenodo + GitHub integration is the reference instance). The runbook is written so an adopter using a different archive service or different release tooling chain can substitute equivalent operations without re-deriving the workflow.

### Requirements

- The skill itself is documentation-only; verification commands inside invoke `git`, optional `python3 -m json.tool` for JSON-LD validation, and optional `cffconvert` for CITATION.cff syntax checking.

### Relationship to companion skills

| Skill | Role | When |
|---|---|---|
| `claude-skill-authorship-strategy` | Judgment framework | Before this skill — determines whether the artifact is in scope for the identifier-federation triplet |
| `claude-skill-context-sync` | Cross-document drift audits | Phase 3 delegates drift detection to this skill |
| `claude-skill-llms-txt-writer` | AI-facing documentation prose | Used during Phase 3 when llms.txt or llms-full.txt needs regeneration |
| `claude-skill-jsonld-knowledge-graph` | JSON-LD knowledge graph | Used during Phase 4 for graph validation |
