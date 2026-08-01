---
title: "Putting QuickSight in Version Control"
date: 2026-08-01
description: "How I added backups, pull requests, validation, and safer publishing around Amazon QuickSight."
summary: "How I added backups, pull requests, validation, and safer publishing around Amazon QuickSight."
tags: ["Platform Engineering", "Amazon QuickSight", "GitOps", "GitHub Actions", "Data Visualization"]
---
### Why the UI was not enough

Our team builds dashboards in Amazon QuickSight. It is useful for moving quickly, but important production work can live almost entirely in a web interface. A small change to a dataset or calculated field can affect several analyses, and undo does not give you the same safety as a known working commit.

I wanted the team to be able to answer ordinary engineering questions about that work. What changed, can someone review it, and can we restore yesterday's version?

### Syncing QuickSight with Git

A GitHub Actions workflow runs every five minutes and exports the definitions for dashboards, analyses, datasets, themes, and visuals. The export is normalized before it is committed so service metadata and ordering do not fill the diff with noise.

If the deployed state no longer matches the repository, the workflow opens a pull request. The team can see the exact change, resolve it, and merge the current state back into version control.

That also gives us dependable backups. If an edit breaks an analysis, we can publish the last working definition instead of rebuilding days of work by hand. People can move faster when an honest mistake is cheap to recover from.

### Catching errors before publishing

Backups do not catch every problem. QuickSight can keep references to fields that no longer exist in the underlying dataset, and some of those errors are easy to miss during a visual review.

I wrote a linter that compares each dashboard and analysis definition with its dataset schema. It catches missing fields and broken relationships before the publish workflow runs. It gives analytics work the same kind of quick feedback I expect from Ruff, mypy, or a compiler.

### Keeping AWS out of the way

Publishing happens through scoped roles in GitHub Actions. Analysts do not need broad IAM access or local AWS credentials to make a dashboard change. They edit the definition, open a pull request, let validation run, and merge the approved result.

Definitions in Git are also much easier for coding agents to understand. An agent can inspect a real dataset, trace a calculated field, update a theme, or help build a dashboard. Its work still comes back as a reviewable diff and passes through the same validation and deployment path as any other change.

This has made QuickSight feel less like a collection of fragile UI state and more like something the team can safely build on.
