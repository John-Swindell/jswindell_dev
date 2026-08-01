---
title: "Machine Learning Research Framework: Crypto Momentum"
date: 2026-01-28
image: "/images/projects/quant_tearsheet_thumb.webp"
description: "A robust backtesting engine built with CatBoost and QuantStats to stress-test volatility strategies against market benchmarks."
tags: ["Machine Learning", "Python", "Quantitative Finance", "CatBoost", "Risk Analysis"]
---
{{< button link="https://jswindell.dev" >}}
Back to Home Page
{{< /button >}}

{{< button link="https://github.com/John-Swindell/machine-learning-research-model" >}}
View Source Code
{{< /button >}}

{{< button link="https://jswindell.dev/blogs" >}}
View More Blogs
{{< /button >}}

<br>
<br>
<br>

### The Objective
In quantitative finance, the goal of a backtesting engine isn't just to validate winning strategies. It is to disqualify risky ones before capital is deployed. This project established a standardized research pipeline to rigorously evaluate momentum-based cryptocurrency strategies using gradient boosting.

### Diagnostic Capabilities
The framework was put to the test evaluating an experimental momentum strategy against a Bitcoin benchmark during the 2023-2025 bull run. The system successfully identified key risk factors that a simple PnL check would have missed.

#### Performance Analysis (Strategy vs. Benchmark)
While the model delivered a strong 104.62% Cumulative Return, the QuantStats diagnostics revealed it was effectively a high-beta play that failed to capture the full upside of the benchmark.

* **Benchmark (BTC) Sharpe:** 1.75 | **Strategy Sharpe:** 1.03
* **Benchmark (BTC) Drawdown:** -23.95% | **Strategy Drawdown:** -57.24%
* **Diagnosis:** The framework flagged that despite positive returns, the strategy carried unacceptable volatility risk (Sortino 1.63 vs BTC 2.91), indicating the need for stricter position sizing during market corrections.

[*(Click to view full QuantStats Report)*](https://model-performance-report.pages.dev/)

### The Infrastructure
To ensure these diagnostics were reliable, the pipeline was architected to avoid common ML pitfalls:

#### 1. Point-in-Time Data Engineering
The pipeline transforms raw OHLCV data into a Feature Matrix using `pandas` and `ta-lib`. Crucially, it uses Winsorization and Robust Scaling inside an expanding window loop. This prevents lookahead bias because the model never sees the global min/max of the dataset, only what was available at that specific moment in history.

#### 2. The Model: CatBoost Regressor
I utilized CatBoost (Gradient Boosting on Decision Trees) for its ability to handle non-linear market regimes.
* **Target:** Predicting the probability of a positive 7-day forward return.
* **Validation:** Utilized TimeSeriesSplit (Walk-Forward Validation) rather than standard K-Fold CV to respect the temporal nature of financial data.

### Key Learnings
The Signal Funnel analysis revealed that while the Extreme Momentum Filter correctly avoided several crashes (e.g., a -14.6% drop in DOGE), the overall portfolio remained too correlated to the broad market. This finding has led to the current development of Market Neutral strategies using the same reliable backtesting infrastructure.

### Technology Stack
* **Core:** Python 3.11, Pandas, NumPy
* **ML:** CatBoost, Scikit-Learn (GridSearchCV)
* **Analysis:** QuantStats, SHAP (Feature Importance), Matplotlib
