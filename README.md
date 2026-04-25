# Out-of-Sample Hierarchical Portfolios: A Review of HEW Improvements over HRP

This repository implements and evaluates **hierarchical clustering-based portfolio allocation methods** in a European market setting, with a particular focus on the out-of-sample robustness of **Hierarchical Risk Parity (HRP)** from López de Prado (2016) and **Hierarchical Equal Weighting (HEW)** from Raffinot (2018).

The project replicates and extends Raffinot’s framework by testing whether his results remain valid under a different empirical context: **European sector, multi-asset and individual equity universes**, over the period **2009–2025**.

---

# Project Objective

Classical mean-variance optimization suffers from well-known instability problems due to the inversion of noisy covariance matrices, particularly when assets are highly correlated — the issue López de Prado refers to as **Markowitz’s Curse**.

Hierarchical portfolio construction offers an alternative by exploiting the dependency structure among assets through clustering, rather than relying solely on traditional covariance optimization.

This project has two objectives:

1. **Theoretical**  
Assess whether hierarchical allocation methods can provide more robust diversification than traditional benchmark portfolios.

2. **Empirical**  
Replicate and test whether the results in Raffinot (2018), especially the relative superiority of some clustering methods such as DBHT and Average Linkage, hold when the framework is transferred to **European markets**.

---

# Methodology

## Portfolio Allocation Models

### Benchmark portfolios
- Equal Weight (**EW**)  
- Inverse Variance Portfolio (**IVP**)  
- Minimum Variance (**MV**)  
- Equal Risk Contribution (**ERC**)  

### Hierarchical portfolios
- Hierarchical Risk Parity (**HRP**)  
- Hierarchical Equal Weighting (**HEW**)

## Clustering Algorithms Tested

The hierarchical models are tested under several clustering schemes:

- **Single Linkage (SL)**  
- **Complete Linkage (CL)**  
- **Average Linkage (AL)**  
- **Ward’s Method**  
- **Directed Bubble Hierarchical Tree (DBHT)**

## Backtesting Framework

Portfolios are estimated using:

- **252-day rolling window**
- **Daily out-of-sample rebalancing**
- Recursive rolling backtest framework

Portfolio evaluation includes:

- Adjusted Sharpe Ratio (**ASR**)  
- Certainty Equivalent Return (**CEQ**)  
- Maximum Drawdown (**MDD**)  
- Average Turnover (**TO**)  
- Sum of Squared Portfolio Weights (**SSPW**)  

Statistical validation is performed through the **Model Confidence Set (MCS)** procedure (Hansen et al., 2011).

For agglomerative clustering, the number of clusters is selected using the **Two Difference Gap Statistic (twodiff)** rather than the Gap Statistic used in Raffinot (2018), due to greater empirical stability in this context.

---

# Data Structure

The analysis is conducted on three European investment universes.

## 1. Sector Universe

Ten **STOXX Europe 600 sector indices**:

- Financials  
- Health Care  
- Technology  
- Utilities  
- Industrials  
- Materials  
- Energy  
- Telecommunications  
- Consumer Staples  
- Consumer Discretionary

---

## 2. Multi-Asset Universe

European equity and sovereign fixed income.

### Equity indices
- Euro Stoxx 50  
- DAX  
- CAC 40  
- IBEX 35  
- Euro Stoxx Small Cap  

### Government bonds
- Germany: 2Y, 5Y, 10Y, 30Y  
- Spain 10Y  
- Italy 10Y  

This setup explicitly captures **core vs peripheral sovereign risk**, a relevant European feature absent from many standard multi-asset studies.

---

## 3. Individual Equity Universe

A representative subset of **70 stocks selected from the STOXX Europe 600**, constructed from a broader universe of 400+ stocks.

Selection is based on:

- Ward clustering partitions  
- Proportional cluster representation  
- Intra-cluster centrality criteria  

This preserves the dependency structure of the full universe while keeping daily rolling backtests computationally tractable.

---

# Main Results (Summary)

Results provide **partial confirmation** of Raffinot (2018):

- Hierarchical portfolios — particularly **HEW variants** — are generally competitive and often outperform traditional benchmarks out of sample.

## Sector Universe
No single dominant model emerges statistically.

- Minimum Variance excels in risk control.
- Some HEW variants improve utility and diversification.
- Ward displays particularly balanced performance.

## Multi-Asset Universe
This universe provides the strongest support for HEW.

- Average Linkage, Complete Linkage and Ward show especially robust results.
- HEW variants compare favorably against both HRP and traditional benchmarks.

## Individual Assets
HEW-SL produces the strongest performance.

- Dominates in ASR and CEQ.
- Survives as the only model in the most stringent MCS.
- Suggests allocation structure may matter at least as much as clustering choice.

**Main conclusion:**  
Hierarchical methods matter, but **no clustering method is universally superior across all universes**. Results are highly context dependent.

---

# Repository Structure

## 1. `Tratamiento de conversión - Eurostoxx 600`
**Currency conversion and preprocessing of STOXX Europe 600 constituents**

- Converts individual equity prices into euros  
- Cleans and aligns raw price histories  
- Prepares the survivorship-consistent equity universe used later in the analysis

---

## 2. `Selección para subconjunto de activos individuales`
**Construction of the representative individual-asset subset**

- Computes clustering structure on the full equity universe  
- Selects the 70-stock representative subset  
- Implements cluster-proportional sampling and centrality-based representative selection

---

## 3. `Formulación de conjuntos y backtest`
**Data construction, portfolio implementation and rolling backtests**

Main research notebook containing:

- Data download and cleaning  
- Return, covariance and correlation estimation  
- Distance matrices and clustering  
- HRP and HEW implementations  
- Benchmark portfolio construction  
- Rolling-window out-of-sample backtesting  
- Portfolio weights and return generation

---

## 4. `Métricas y análisis`
**Performance evaluation and statistical validation**

- Computes all performance metrics  
- Portfolio concentration and turnover analysis  
- Model Confidence Set (MCS) procedure  
- Comparative analysis of out-of-sample results

---

# Data Sources

Data used in this project comes from two primary sources.

## Refinitiv Workspace
Used for:

- STOXX Europe 600 sector indices  

## Yahoo Finance (via yfinance)
Used for:

- Multi-asset dataset  
- STOXX Europe 600 constituents  

## Data Frequency
- Daily adjusted closing prices  
- Returns computed from synchronized daily series  
- Common sample: **January 2009 – December 2025**

## Notes on Data Construction
- All individual equities are converted into **EUR** for consistency.
- The individual-stock universe includes assets with sufficient price history from the start of the sample.
- The 70-stock subset is a representative reduced universe constructed from clustering structure, not the full STOXX Europe 600 universe.
- The individual equity universe may contain **survivorship bias**, consistent with the caveat discussed in Raffinot (2018).

---

# Environment

Main libraries used:

- Python 3  
- pandas  
- numpy  
- scipy  
- networkx  
- matplotlib  
- seaborn  
- Riskfolio-Lib  
- arch

Implementation developed in **Google Colab**.

---

# Key References

- Markowitz, H. “Portfolio Selection.” *The Journal of Finance*, Vol. 7, No. 1 (1952), pp. 77-91.

- López de Prado, M. “Building Diversified Portfolios That Outperform Out of Sample.” *The Journal of Portfolio Management*, Vol. 42, No. 4 (2016a), pp. 59-69.

- Raffinot, T. “Hierarchical Clustering-Based Asset Allocation.” *The Journal of Portfolio Management*, Vol. 44, No. 2 (2018), pp. 89-99.

- Mantegna, R.N. “Hierarchical Structure in Financial Markets.” *The European Physical Journal B: Condensed Matter and Complex Systems*, Vol. 11, No. 1 (1999), pp. 193-197.

- Ward, J.H. “Hierarchical Grouping to Optimize an Objective Function.” *Journal of the American Statistical Association*, Vol. 58, No. 301 (1963), pp. 236-244.

- DeMiguel, V., L. Garlappi, and R. Uppal. “Optimal Versus Naive Diversification: How Inefficient Is the 1/N Portfolio Strategy?” *The Review of Financial Studies*, Vol. 22, No. 5 (2009), pp. 1915-1953.

- Pezier, J., and A. White. “The Relative Merits of Alternative Investments in Passive Portfolios.” *The Journal of Alternative Investments*, Vol. 10, No. 4 (2008), pp. 37-39.

- Levy, M. “Measuring Portfolio Performance: Sharpe, Alpha, or the Geometric Mean?” Unpublished paper, 2016.

- Goetzmann, W.N., and A. Kumar. “Equity Portfolio Diversification.” *Review of Finance*, Vol. 12, No. 3 (2008), pp. 433-463.

- Hansen, P., A. Lunde, and J. Nason. “The Model Confidence Set.” *Econometrica*, Vol. 79, No. 2 (2011), pp. 453-497.

- Tibshirani, R., G. Walther, and T. Hastie. “Estimating the Number of Clusters in a Data Set via the Gap Statistic.” *Journal of the Royal Statistical Society: Series B (Statistical Methodology)*, Vol. 63, No. 2 (2001), pp. 411-423.

- Tumminello, M., T. Aste, T. Di Matteo, and R. Mantegna. “A Tool for Filtering Information in Complex Systems.” *Proceedings of the National Academy of Sciences of the United States of America*, Vol. 102, No. 30 (2005), pp. 11-23.

- Aste, T., W. Shaw, and T.D. Matteo. “Correlation Structure and Dynamics in Volatile Markets.” *New Journal of Physics*, Vol. 12, No. 8 (2010), pp. 5-9.

- Musmeci, N., T. Aste, and T. Di Matteo. “Relation between Financial Market Structure and the Real Economy: Comparison Between Clustering Methods.” *PLoS One*, Vol. 10, No. 3 (2015), pp. 201-210.

- Cajas, D. “Advanced Portfolio Optimization: A Cutting-edge Quantitative Approach.” Springer, Cham (2025).
---

# Reproducibility

This repository is intended as a research replication framework.

All portfolio construction, backtesting and performance evaluation can be reproduced through the notebooks provided.

---

## License

This project is released under the **MIT License**.
