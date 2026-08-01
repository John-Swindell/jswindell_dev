---
title: "Testing a Crypto Momentum Model"
date: 2026-01-28
image: "/images/projects/quant_tearsheet_thumb.webp"
description: "A backtesting project that helped me reject a profitable-looking model once its risk showed up."
summary: "A backtesting project that helped me reject a profitable-looking model once its risk showed up."
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

### What I wanted to test

This project started with a simple question. Could a momentum model beat holding Bitcoin without taking on much more risk?

I built a repeatable research pipeline around CatBoost and tested it against Bitcoin during the 2023 to 2025 market run. The important part was not finding a profitable chart. It was building enough diagnostics to tell me when a profitable result was still a bad trade.

### Building the backtest

The pipeline turns OHLCV data into a feature matrix with pandas and TA-Lib. Scaling and outlier handling happen inside an expanding window, so each prediction only uses information that would have existed at that point in the test.

I used walk-forward validation instead of random cross-validation because market data has an order that cannot be shuffled away. CatBoost then estimated the chance of a positive return over the next seven days.

### What the numbers showed

The strategy returned 104.62 percent, which looked promising on its own. The comparison with Bitcoin told a different story.

| Metric | Strategy | Bitcoin |
|---|---:|---:|
| Sharpe ratio | 1.03 | 1.75 |
| Maximum drawdown | -57.24% | -23.95% |
| Sortino ratio | 1.63 | 2.91 |

The model made money, but it did so with much worse downside and less return for each unit of risk. It was mostly a more fragile way to stay exposed to the same market.

[View the full QuantStats report](https://model-performance-report.pages.dev/)

### What I kept from it

The signal filters did avoid some sharp individual drops, including a 14.6 percent decline in DOGE, but the portfolio was still too correlated with the broader market. I did not treat the positive return as a successful model.

The useful result was the research framework itself. It made the weakness obvious and gave me a cleaner base for testing market-neutral ideas, where the model has to find a signal instead of relying on the whole market moving up.
