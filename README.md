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

SmartStrategy is a quantitative trading framework that leverages predictive modeling and signal-based logic to dynamically allocate capital among selected equities. The system employs individual LSTM models to forecast daily closing prices for Apple (AAPL), Tesla (TSLA), and Amazon (AMZN), and uses directional predictions to guide trades. Performance is benchmarked against the S&P 500 index.

<a name="top"></a>
<div align="center">
  <img src="./figures/Comparison.png" style="width: 100%;"/>
</div>

## Overview

The dataset spans from January 2019 to December 2024, with financial data obtained via Yahoo Finance (or local CSV alternatives when unavailable). The strategy was designed with a training window from 2019–2021 and a test window from 2021–2024.

Each stock is modeled independently using an LSTM neural network implemented in PyTorch, configured with SiLU activations and five fully connected layers. Training employs the Adam optimizer, along with a learning rate scheduler and early stopping based on validation loss. A 60-day rolling window is used to predict next-day closing prices.

Model performance is evaluated using:

* Mean Absolute Error (MAE)
* Mean Absolute Percentage Error (MAPE)
* Directional Accuracy

## Model Highlights

* Separate LSTM models are trained for each stock.
* Model predicts next-day closing prices based on a 60-day rolling window.
* Achieves high test accuracy:
  * Apple: 97.36%
  * Tesla: 94.50%
  * Amazon: 95.80%

## Strategy Logic

The strategy computes one-day-ahead predicted changes for each stock and derives percentage-based signals. It selects the stock with the highest absolute predicted change to form a directional trade signal. To reduce noise and overtrading, signals are:

* Smoothed using a centered rolling mean
* Thresholded (±1%) to filter insignificant predictions
* Held for a fixed 5-day period, regardless of intermediate signal changes

The final position (long, short, or hold) determines which stock is held and for how long. Returns are calculated based on realized price changes during the holding period.

## Performance Summary

The table below compares SmartStrategy to a passive SPY buy-and-hold benchmark over the test period:

<a name="top"></a>
<div align="center">
  <img src="./figures/Result.png" style="width: 100%;"/>
</div>

A log-scaled equity curve visualization further illustrates the compounding advantage of SmartStrategy over the benchmark and the individual constituent stocks.

<div align="center">

<table>
  <thead>
    <tr>
      <th>Strategy</th>
      <th>Total Return</th>
      <th>Annualized Return</th>
      <th>Sharpe Ratio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>SmartStrategy</strong></td>
      <td>5088.22%</td>
      <td>169.44%</td>
      <td>2.26</td>
    </tr>
    <tr>
      <td>SPY (Buy &amp; Hold)</td>
      <td>59.62%</td>
      <td>12.45%</td>
      <td>0.73</td>
    </tr>
  </tbody>
</table>

</div>

## Key Features

* Individual LSTM Models: Trained independently for AAPL, TSLA, and AMZN
* Signal-Based Allocation: Selects one stock per day based on predicted movement
* Noise Control: Applies rolling average smoothing and signal thresholds
* Position Management: Enforces a 5-day fixed holding period
* Backtested Evaluation: Cumulative returns, Sharpe ratio, and directional metrics
* Benchmark Comparison: Against SPY and individual buy-and-hold strategies
