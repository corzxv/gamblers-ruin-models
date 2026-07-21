# Gambler's Ruin Models

![R](https://img.shields.io/badge/R-276DC3?style=flat-square&logo=r&logoColor=white)
![Monte Carlo](https://img.shields.io/badge/Method-Monte%20Carlo-555555?style=flat-square)
![Markov Chains](https://img.shields.io/badge/Model-Markov%20Chains-6A5ACD?style=flat-square)

A study of the classical **Gambler's Ruin problem** using absorbing Markov chains, Monte Carlo simulation, and an application to historical financial data.

## About the project

The Gambler's Ruin problem describes a player whose capital changes by one unit after each independent round:

- the player gains one unit with probability `p`;
- the player loses one unit with probability `q = 1 - p`;
- the process ends when the capital reaches `0` or a predefined target `N`.

The two absorbing states are:

- `0`: ruin;
- `N`: victory.

This project compares theoretical results with Monte Carlo simulations and applies the same framework to daily price movements of PETR4 shares between 2020 and 2024.

The objective is to explore:

- random walks;
- absorbing Markov chains;
- ruin and victory probabilities;
- expected absorption time;
- Monte Carlo validation;
- hypothesis testing;
- a simplified application to financial data.

## Project components

The repository contains two main analyses.

| Script | Description | Generated figures |
|---|---|---|
| `simulacao_ruina.R` | Simulates random trajectories and compares empirical ruin probabilities and expected durations with their theoretical values. | `fig_trajetorias.png`, `fig_ruin_prob.png`, `fig_duration.png` |
| `aplicacao_real.R` | Applies the model to PETR4 daily returns from 2020 to 2024, estimates the probability of a positive return, tests the fair-game hypothesis, and compares empirical and theoretical ruin probabilities. | `fig_real_ruin.png`, `fig_real_traj.png` |

## Mathematical framework

Let:

- `x` be the initial capital, with `0 < x < N`;
- `N` be the target capital;
- `p` be the probability of gaining one unit;
- `q = 1 - p` be the probability of losing one unit;
- `r = q / p`.

### Victory probability

For a fair random walk, where `p = q = 0.5`, the probability of reaching `N` before ruin is:

$$
P(\text{victory} \mid X_0 = x) = \frac{x}{N}
$$

For `p ≠ q`:

$$
P(\text{victory} \mid X_0 = x)
=
\frac{1-r^x}{1-r^N}
$$

### Ruin probability

The probability of reaching `0` before `N` is the complement of the victory probability.

For `p = q = 0.5`:

$$
P(\text{ruin} \mid X_0 = x)
=
1-\frac{x}{N}
$$

For `p ≠ q`:

$$
P(\text{ruin} \mid X_0 = x)
=
\frac{r^x-r^N}{1-r^N}
$$

### Expected duration

For a fair random walk:

$$
E[T \mid X_0=x] = x(N-x)
$$

For `p ≠ q`:

$$
E[T \mid X_0=x]
=
\frac{N \cdot P(\text{victory} \mid X_0=x)-x}{p-q}
$$

Here, `T` represents the number of rounds required for the process to reach either absorbing state.

## Monte Carlo simulation

The simulation repeatedly generates random walks until each trajectory reaches:

- ruin at `0`; or
- victory at `N`.

The empirical estimates are then compared with the theoretical formulas for:

- probability of ruin;
- probability of victory;
- expected absorption time.

The comparison helps verify that the simulation converges toward the analytical results as the number of repetitions increases.

## Financial application

The second script applies the framework to PETR4 daily adjusted prices between 2020 and 2024.

Daily returns are simplified into two possible outcomes:

- positive return: `+1`;
- negative return: `-1`.

The empirical probability of a positive return is estimated as:

$$
\hat{p}
=
\frac{\text{number of positive returns}}
{\text{number of non-zero returns}}
$$

The analysis then:

1. downloads PETR4 historical market data;
2. calculates daily returns;
3. converts each return into an upward or downward movement;
4. estimates `p`;
5. tests whether the process is compatible with a fair game, where `p = 0.5`;
6. simulates capital trajectories using the estimated probability;
7. compares empirical and theoretical ruin probabilities.

## Important interpretation

The financial application is a simplified illustration of the Gambler's Ruin framework.

It assumes that:

- daily movements are independent;
- the probability of an increase remains constant;
- all positive and negative returns have the same magnitude;
- transaction costs and volatility magnitude are ignored.

These assumptions do not represent the full behavior of financial markets.

Therefore, the project should be interpreted as an academic application of stochastic-process concepts, not as an investment strategy or financial forecasting model.

## Running the project

### Requirements

- R 4.1 or later;
- internet access for the PETR4 analysis.

Required packages include:

- `ggplot2`;
- `dplyr`;
- `scales`;
- `quantmod`;
- `zoo`.

Missing packages are installed automatically by the scripts.

### Run the simulated study

```bash
Rscript simulacao_ruina.R
```

### Run the financial application

```bash
Rscript aplicacao_real.R
```

The second script requires internet access to download PETR4 historical data.

## Outputs

The simulated study generates:

| Figure | Description |
|---|---|
| `fig_trajetorias.png` | Examples of simulated capital trajectories until ruin or victory. |
| `fig_ruin_prob.png` | Comparison between theoretical and empirical ruin probabilities. |
| `fig_duration.png` | Comparison between theoretical and empirical expected durations. |

The financial application generates:

| Figure | Description |
|---|---|
| `fig_real_ruin.png` | Comparison between theoretical and empirical ruin probabilities using the estimated PETR4 movement probability. |
| `fig_real_traj.png` | Simulated trajectories based on the empirical PETR4 probability. |

## Implementation details

Several improvements were applied to make the scripts more efficient and reusable:

- ruin and duration are calculated during the same Monte Carlo simulation;
- theoretical functions are vectorized over different initial-capital values;
- plotting logic is organized into reusable helper functions;
- simulation results are stored in structured data frames;
- theoretical and empirical results are displayed together for direct comparison;
- random seeds are used where appropriate to improve reproducibility.

## Technologies and concepts

- **R**
- **Monte Carlo simulation**
- **Markov chains**
- **Random walks**
- **Absorbing states**
- **Hypothesis testing**
- **Financial time series**
- **Data visualization**
- **quantmod**
- **ggplot2**

## Limitations

The main limitations of the project are:

- the random walk uses fixed movements of `+1` and `-1`;
- observations are treated as independent;
- the probability `p` is assumed to remain constant;
- the financial application ignores return magnitude;
- transaction costs, dividends, liquidity, and market regimes are not considered;
- theoretical results rely on the assumptions of the classical Gambler's Ruin model.

## Possible improvements

Future versions could include:

- confidence intervals for Monte Carlo estimates;
- convergence analysis for different simulation sizes;
- sensitivity analysis for `p`, `x`, and `N`;
- unequal gain and loss sizes;
- time-varying transition probabilities;
- comparison across multiple financial assets;
- bootstrap estimation of the movement probability;
- regime-dependent models;
- an interactive application for changing simulation parameters.

## Academic context

This repository documents an applied study of stochastic processes, combining analytical probability results, numerical simulation, and a simplified application to real-world data.

The project was designed to connect the theoretical behavior of absorbing Markov chains with empirical results obtained through Monte Carlo simulation.

## Author

**Felipe Cordeiro**

Data & BI Analyst and Statistics undergraduate focused on analytics, statistical modeling, machine learning, and data science.

[LinkedIn](https://www.linkedin.com/in/felipemcordeiro/) · [GitHub](https://github.com/corzxv)
