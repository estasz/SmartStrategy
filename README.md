<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<a name="top"></a>
<div align="center">
  <h1>SmartStrategy</h1>
</div>

---

<div align="center">
   <a href="mailto:erik.staszewski@gmail.com"><b>Email Me</b></a> | <a href="https://www.linkedin.com/in/estasz/"><b>My LinkedIn</b></a></b></a>
</div>

This project, SmartStrategy, supports quantitative trading to outperform S&P 500 buy-and-hold strategies using a signal-driven method. The strategy uses stocks like Apple, Amazon, and Tesla.

## Overview

* **Data Range:** 2019–2024
  * **Training Window:** 2019–2021  
  * **Test Window:** 2021–2024  
* **Data Source:** Yahoo Finance
* **Model Architecture:** LSTM (PyTorch) with SiLU activations and multi-layer fully connected heads  
* **Optimization:** Adam + LR scheduler + Early stopping
* **Evaluation Metrics:** MAE, MAPE, directional accuracy

### Model Highlights

* Separate LSTM models are trained for each stock.
* Model predicts next-day closing prices based on a 60-day rolling window.
* Achieves high test accuracy:
  * Apple: 97.36%
  * Tesla: 94.50%
  * Amazon: 95.80%

### Strategy Logic

* Evaluates predicted percentage change across all stocks
* Selects the stock with the strongest signal (up or down)
* Applies smoothing and thresholds to reduce noise
* Executes trades with fixed holding periods
* Compounds returns based on directional signal accuracy

### Performance Summary

| Strategy             | Total Return | Annualized Return | Sharpe Ratio |
|----------------------|--------------|-------------------|--------------|
| **SmartStrategy**    | 5088.22%     | 169.44%           | 2.26         |
| SPY (Buy & Hold)     | 59.62%       | 12.45%            | 0.73         |

> [!NOTE]
> SmartStrategy outperformed all benchmarks in both absolute and risk-adjusted returns.

### Key Features

- Clean, normalized data processing
- Individual training + backtesting per stock
- Aggregated strategy selects best-performing signals
- Visual comparison of predicted vs actual prices
- Equity curve and portfolio growth visualization
- Comparison to buy-and-hold benchmarks
