# Risk and Return Trade-off in Chinese Market: A CGO Perspective

This repository contains the code and data processing pipeline for replicating and extending the study by **Wang, Yan, and Yu (2016)** in the Chinese A-share market. 

We investigate whether **Capital Gains Overhang (CGO)** conditions the risk-return trade-off in the **CSI 500** universe from **2014 to 2023**.

## 📄 Abstract

Existing studies suggest that CGO strongly conditions the risk-return relation in the U.S. market: high-CGO firms exhibit a positive risk-return tradeoff, while low-CGO firms show an inverted pattern. 

This project tests this behavioral mechanism in China's emerging market, characterized by high retail participation and unique trading constraints (e.g., T+1 trading, price limits). Using a turnover-based CGO measure and six risk proxies (Beta, Idiosyncratic Volatility, Return Volatility, Cash-flow Volatility, Age, and Analyst Dispersion), we employ single sorts, double sorts, and Fama-MacBeth regressions.

**Key Findings:**
*   Confirmed a robust **low-risk anomaly**: high-volatility portfolios consistently earn lower returns.
*   **No statistically significant interaction** found between CGO and risk proxies across all specifications.
*   The CGO-dependent risk-return relation documented in the U.S. does not generalize to the CSI 500 during this period.

## 📂 Repository Structure

The project is organized into a sequential pipeline of Jupyter Notebooks:

| Step | Notebook | Description |
| :--- | :--- | :--- |
| **01** | `s01cgo.ipynb` | **Data Cleaning & CGO Calculation**: Processes daily price/volume data, handles suspensions/delistings, and computes the turnover-based Capital Gains Overhang (CGO) using a recursive reference price formula. |
| **02** | `s02Mom.ipynb` | **Momentum Factors**: Calculates various momentum indicators (MOM -1,0; -12,-1; -36,-12; -11,-2) based on monthly returns. |
| **03** | `s03BM_mcap.ipynb` | **Fundamental Data**: Processes Book-to-Market (BM) ratio and Market Capitalization (MCAP), converting them to logarithmic forms (LOGBM, LOGME). |
| **04** | `s04risk_factors.ipynb` | **Risk Proxies Construction**: Calculates 6 risk metrics: CAPM Beta, Idiosyncratic Volatility (IVOL), Return Volatility (RETVOL), Cash Flow Volatility (CFVOL), Firm Age, and Analyst Forecast Dispersion (DISP). |
| **11** | `s11add_BM_mcap_mom.ipynb` | **Factor Merging**: Merges fundamental factors and momentum factors into a unified dataset. |
| **12** | `s12merge_all.ipynb` | **Final Dataset Assembly**: Combines all risk proxies, CGO, and control variables. Performs winsorization (1%-99%) and Z-score normalization. Constructs the Mispricing Score. |
| **21** | `s21single_sort.ipynb` | **Single Portfolio Sorts**: Sorts stocks into quintiles based on risk proxies and CGO to analyze univariate return spreads. |
| **22** | `s22double_sort.ipynb` | **Double Portfolio Sorts**: Performs dependent double sorts (first by CGO, then by Risk) to test the conditional risk-return relationship. |
| **23** | `s23fama_macbeth.ipynb` | **Fama-MacBeth Regressions**: Runs cross-sectional regressions controlling for size, value, momentum, turnover, and testing interactions with Underreaction to News and Disposition Effect. |
| **31** | `s31disposition.ipynb` | **Disposition Effect Analysis**: Specific analysis focusing on the disposition effect mechanism. |

## 🛠️ Methodology

### Data Source
*   **Universe**: CSI 500 constituents (dynamic).
*   **Period**: Jan 2014 - Dec 2023.
*   **Frequency**: Monthly rebalancing.

### Key Variables
1.  **CGO (Capital Gains Overhang)**: Calculated using a turnover-based reference price (Grinblatt and Han, 2005) with a lookback window suitable for the high-turnover Chinese market.
2.  **Risk Proxies**:
    *   `beta_MKT`: CAPM Beta (rolling 5-month).
    *   `IVOL`: Idiosyncratic Volatility relative to FF3 factors.
    *   `RETVOL`: Total Return Volatility.
    *   `CFVOL`: Cash Flow Volatility (earnings/assets).
    *   `AGE`: Reciprocal of firm age (1/Age).
    *   `DISP`: Analyst Forecast Dispersion.
3.  **Controls**: Size (LOGME), Value (LOGBM), Momentum (various horizons), Turnover.

### Models
*   **Portfolio Sorts**: Newey-West adjusted t-statistics for long-short portfolios.
*   **Fama-MacBeth Regression**:
    $$ R_{i,t+1} = \alpha + \beta_1 CGO_{i,t} + \beta_2 Risk_{i,t} + \beta_3 (CGO_{i,t} \times Risk_{i,t}) + \gamma Controls_{i,t} + \epsilon_{i,t+1} $$

## 📊 Results Summary

*   **Single Sorts**: Strong negative relationship between volatility (IVOL/RETVOL) and future returns.
*   **Double Sorts**: The "Low Risk Anomaly" exists in both high and low CGO groups. Unlike the US market, high CGO does not flip the negative risk-return relation to positive.
*   **Regressions**: The interaction term `CGO * Proxy` is statistically insignificant across all specifications, suggesting CGO is not a primary driver of risk pricing in this sample.

## 👥 Authors

*   Li Mingming
*   Dou Boshu
*   Zhong Zewei

## 📜 References

*   Wang, H., Yan, J., & Yu, J. (2017). *Reference-dependent preferences and the risk-return trade-off*. Journal of Financial Economics.
*   Grinblatt, M., & Han, B. (2005). *Prospect theory, mental accounting, and momentum*. Journal of Financial Economics.
*   Frazzini, A., & Pedersen, L. H. (2014). *Betting against beta*. Journal of Financial Economics.

---
*Disclaimer: This project is for academic and research purposes only and does not constitute financial advice.*
