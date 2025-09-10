# Study on one-factor managed portfolio vs common-volatility managed portfolio

## 💡Introduction

### Return of a Portfolio

The return of a portfolio with a collection of assets $$i$$'s at time $$t$$ is given by

$$r_t = \sum_i w_i r_{i,t}$$

where $$w_i$$ is the weight of asset $$i$$ in the portfolio and $$r_{i,t}$$ is its return at time $$t$$.  

### 'One-factor' Approach

When managing a portfolio, the weight $$w_i$$ is adjusted dynamically over time. A common strategy to modulate $$w_i$$ is based on a quantity $$x$$ determined by some volatility $$\sigma_{i,t}$$ of the asset $$i$$ at time $$t$$. In equation, it looks

$$r_t = \sum_i x_{i,t-1} w_i r_{i,t}$$  

where $$x_{i,t-1}$$ is a function of $$\sigma_{i,t-1}$$ adjusting the weight $$w_i$$. The volatility can be the variance of return, momentum, etc. of asset $$i$$. Each asset is rebalanced based on the volatility of its own. This is known as the 'one-factor' (i.e. per-asset) approach. 

The drawback of this approach is that there is usually noisy measurement in the individual asset's volatility, suggesting big swings in position size. 

### 'Common-Volatility' Approach

DeMIGUEL *et al.* from London Business School suggested in their paper [link](https://lbsresearch.london.edu/id/eprint/3716/1/The%20Journal%20of%20Finance%20-%202024%20-%20DeMIGUEL%20-%20A%20Multifactor%20Perspective%20on%20Volatility%E2%80%90Managed%20Portfolios.pdf) that the risk-return trade-off should be determined by the overall market turbulence instead of the single asset's idiosyncratic wiggles. In other words, we should use a common marker-wide volatility for all the assets when adjusting the weights. More explicitly, we should consider a common $$x$$ derived from the market volatility $$\sigma_M$$ for all assets as below:

$$r_t = x_{t-1} \sum_i w_i r_{i,t}$$

This avoids chasing idiosyncratic noise per name, suppresses turnover, and captures the stylized fact that aggregate risk rises in bad times. 

## 💡Work in this Repo

### Methods

In order to test the effectiveness of 'Common-Volatility' approach suggested in the paper, in this repo, I conducted an experiment using historical data on a porfolio consisting the 'Magnificent 7' stocks (Alphabet, Amazon, Apple, Meta Platforms, Microsoft, Nvidia, and Tesla). Other parameters I used are as follows:

- <mark>Volatility $$\sigma$$</mark>: Monthly Variance of Asset Return (sample variance over daily return in a month)
- <mark>risk-free rate $$r_f$$</mark>: 3-Month Treasury Bill monthly from FRED
- Market value: S&P 500 Index
- Training period (for rescaling factor): 2023-2024
- Evaluation period: 2025 Jan-Aug (8 months)

Using the 'One-factor' and 'Common-volatility' approaches (as well as an equal weight approach as reference), I computed CARG (Compounded Annual Growth Rate), AnnVol (Annualized Volatility), Sharpe ratio and MaxDrawdown (Worst peak-to-trough loss) metrics of the portfolio in the evaluation period and compared the performance of different approaches.

### Results

The comparison of the performance of two different approaches on the 'Magnificent 7' portfolio in the evaluation period 2025 Jan-Aug is as follows:

<table style="border: 1px solid black;border-collapse: collapse;">
  <thead>
    <tr>
      <th style="border: none; padding: 5px;"></th>
      <th style="border: none; padding: 5px;">Months</th>
      <th style="border: none; padding: 5px;">CAGR</th>
      <th style="border: none; padding: 5px;">AnnVol</th>
      <th style="border: none; padding: 5px;">Sharpe</th>
      <th style="border: none; padding: 5px;">MaxDrawdown</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: none; padding: 5px;">OneFactor:</td>
      <td style="border: none; padding: 5px;">8</td>
      <td style="border: none; padding: 5px;">0.0811</td>
      <td style="border: none; padding: 5px;">0.2348</td>
      <td style="border: none; padding: 5px;">0.2601</td>
      <td style="border: none; padding: 5px;">-0.1627</td>
    </tr>
    <tr>
      <td style="border: none; padding: 5px;">CommonVol:</td>
      <td style="border: none; padding: 5px;">8</td>
      <td style="border: none; padding: 5px;">0.1072</td>
      <td style="border: none; padding: 5px;">0.1787</td>
      <td style="border: none; padding: 5px;">0.4195</td>
      <td style="border: none; padding: 5px;">-0.1213</td>
    </tr>
  </tbody>
</table>

<br>
It can be seen that the one-factor approach underperformed on both risk and return.

## 📜Usage

### Directory structure
```
├─ 01_collect_data.ipynb # collect market data and stock prices for the Mag 7 portfolio  
├─ 02_build_signals_and_strategy.ipynb # 'training' script for the period 2023-2024
├─ 03_evaluation.ipynb # 'evaluation' script for 2025 Jan-Aug
│ 
├─ data # directory storing data collected. It will be created after running 01_collect_data.ipynb. 
│  # Data includes stock prices, risk-free rates and market info.
│ 
└─ requirements.txt
```

The environment can be set up by `pip install -r requirements.txt`

After setting up the environment, you can run the .ipynb's in the following order
- 01_collect_data.ipynb
- 02_build_signals_and_strategy.ipynb
- 03_evaluation.ipynb


