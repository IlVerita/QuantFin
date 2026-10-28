# Statistical Analysis of S&P 500 Returns

## Overview

This project investigates the statistical properties of daily S&P 500 returns, with particular emphasis on the limitations of Gaussian models, temporal dependence, and volatility clustering.

The objective is to develop and evaluate probabilistic models for financial returns using statistical inference, hypothesis testing, and numerical methods.

The analysis is conducted in Python using historical S&P 500 data and is developed progressively, from a simple Gaussian independent and identically distributed (i.i.d.) model toward more realistic models with time-varying volatility.

## Research Objectives

The main questions addressed by the project are:

- Are daily S&P 500 returns adequately described by a Gaussian distribution?
- Are returns independently distributed over time?
- Do squared returns exhibit significant temporal dependence?
- Can models with time-varying conditional volatility explain the observed statistical properties?
- How can the adequacy of these models be evaluated using statistical hypothesis tests?

## Methodology

### 1. Gaussian i.i.d. Model

We initially consider the hypothesis

$$
H_0: \quad R_t \overset{\mathrm{iid}}{\sim} \mathcal{N}(\mu,\sigma^2).
$$

The parameters are estimated from historical daily returns.

To assess the goodness of fit, we use Pearson's chi-squared statistic, with equiprobable bins determined by the fitted Gaussian distribution.

The null distribution of the test statistic is estimated through a **parametric bootstrap**, re-estimating the Gaussian parameters for each simulated sample.

This approach allows us to evaluate the adequacy of the Gaussian i.i.d. hypothesis without relying exclusively on the asymptotic chi-squared approximation.

### 2. Time-Varying Volatility

We then consider a Gaussian model with time-varying conditional volatility:

$$
R_t = \mu + \Sigma_t Z_t,
\qquad
Z_t \overset{\mathrm{iid}}{\sim} \mathcal{N}(0,1),
$$

where the positive random variable $\Sigma_t$ depends on information available at time $t-1$.

Under this model,

$$
\mathbb{E}[R_t\mid\mathcal{F}_{t-1}] = \mu,
$$

$$
\operatorname{Var}(R_t\mid\mathcal{F}_{t-1}) = \Sigma_t^2.
$$

Although returns are theoretically uncorrelated at nonzero time lags, squared returns may exhibit nonzero autocorrelations.

We investigate this behavior through the sample autocorrelation functions (ACFs) of daily returns and squared daily returns.

### 3. Hypothesis Testing for Temporal Correlations

To evaluate whether the observed autocorrelations of returns are statistically significant, we construct a joint Wald test for the first $K$ autocovariances.

The null hypothesis is

$$
H_0:\quad \rho_R(\kappa)=0,
\qquad \kappa=1,\ldots,K.
$$

The test statistic is

$$
T =
n\,\overline{\mathbf G}^{\,T}
\widehat{\Omega}^{-1}
\overline{\mathbf G},
$$

where $\overline{\mathbf G}$ is the vector of sample autocovariance estimators and $\widehat{\Omega}$ estimates their asymptotic covariance matrix.

We use a heteroskedasticity and autocorrelation consistent (HAC) covariance estimator based on the Newey–West methodology.

Under suitable regularity conditions, the Wald statistic follows an asymptotic chi-squared distribution under the null hypothesis.

## Preliminary Findings

The analysis so far indicates:

- **Gaussian i.i.d. model:** the goodness-of-fit analysis provides strong evidence against the Gaussian i.i.d. hypothesis.
- **Return autocorrelations:** the sample ACF displays fluctuations around zero. A preliminary HAC-robust Wald test does not reject the joint null hypothesis of zero autocorrelation for the first ten lags.
- **Squared-return autocorrelations:** the sample ACF exhibits persistent positive correlations across multiple trading-day lags, consistent with volatility clustering.

These findings motivate the investigation of models with dynamically evolving conditional variance.

The results remain preliminary, and further robustness checks and model diagnostics are required.

## Project Structure

`01_Exploraotory_analysis.ipynb` — Data exploration, Gaussian goodness-of-fit analysis, autocorrelation functions, and statistical hypothesis tests.

Additional notebooks may be added as the analysis progresses.

## Technologies

- **Python** — Statistical analysis and numerical computation
- **NumPy / Pandas** — Data processing and manipulation
- **SciPy** — Probability distributions and statistical inference
- **Statsmodels** — Time-series analysis and statistical diagnostics
- **Matplotlib** — Data visualization
- **Jupyter Notebook** — Reproducible computational analysis

## Getting Started

Clone the repository:

`git clone https://github.com/IlVerita/QuantFin.git`

Install the main Python dependencies:

`pip install numpy pandas scipy matplotlib statsmodels jupyter`

Open the notebook in Jupyter or Visual Studio Code and execute the analysis cells in order. Additional dependencies may be needed for the notebook's data-loading procedure.

## Future Work

The planned extensions include:

- Statistical testing of temporal dependence in squared returns.
- ARCH and GARCH models for conditional volatility.
- Parameter estimation using maximum likelihood.
- Model diagnostics based on standardized residuals.
- Alternative conditional distributions, including Student's t.
- Comparison of volatility models and their predictive performance.

## Acknowledgements

ChatGPT (OpenAI) has been used as a supplementary tool for discussing statistical concepts, reviewing mathematical derivations, and assisting with Python implementation.

The project is developed as an independent study of statistical methods applied to financial time series.

## Disclaimer

This repository is intended for educational and research purposes. It does not provide financial or investment advice.