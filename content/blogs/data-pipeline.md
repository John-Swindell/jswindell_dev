---
title: "Building a Crypto Data Pipeline"
date: 2026-01-30
image: "/images/projects/diagram_simple_transparent.webp"
description: "A Dockerized GCP pipeline for collecting cleaner historical crypto data without hammering external APIs."
summary: "A Dockerized GCP pipeline for collecting cleaner historical crypto data without hammering external APIs."
source: "https://github.com/John-Swindell/data-engineering-etl-pipeline"
tags: ["Data Engineering", "GCP", "Python", "Architecture"]
---
### The data problem

I built this pipeline for research that needed a believable picture of the crypto market at different points in time. Using only the assets that exist today makes old backtests look better than they should because failed projects disappear from the sample.

Timing creates another problem. A metric may describe an earlier event but only become available later. If a model uses it too soon, the backtest is quietly learning from the future.

### The pipeline

The pipeline rebuilds a top-200 asset universe for each historical date, then collects market, on-chain, and social data for that universe.

[![Pipeline Architecture](/images/projects/pipeline_architecture.webp)](/images/projects/pipeline_architecture.webp)

Raw API responses are stored before any cleanup. A second layer standardizes schemas, removes bad outliers, and prepares the data for feature work. Keeping those stages separate makes it possible to inspect the original response when a transformed value looks wrong.

To reduce unnecessary API calls, the pipeline checks a local cache first and a shared Google Cloud Storage cache second. That reduced external requests by more than 90 percent and made repeated research runs much faster.

### Keeping historical data honest

Features are timestamped according to when the information was actually available, not only when the underlying event happened. The pipeline also combines related liquidity, such as wrapped and native versions of the same asset, without counting market capitalization twice.

A schema check runs on pull requests and flags unexpected changes in previously collected history. That gives us a chance to investigate an upstream revision before it changes a model result.

### Repeatable runs

The pipeline runs in Docker so local and production jobs use the same Python environment. A small orchestration script runs universe generation, ingestion, validation, and transformation in order. If validation fails, the job stops instead of passing questionable data downstream.

The result was a shared research dataset that was faster to work with and much less likely to reward a model for information it could not have known at the time.
