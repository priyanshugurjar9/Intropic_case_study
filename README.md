# US Index Case Study - Section D

**Author:** Priyanshu Gurjar  
**Purpose:** Intropic Research Analyst Case Study

## Project Overview

This repository contains the complete Section D submission for the Intropic
Research Analyst Case Study. It combines:

- qualitative research into FTSE Russell index methodology;
- a reconstruction of Linde plc's shares, free float and country
  classification around the 2020 and 2021 June Reconstitutions; and
- a Python model of the hypothetical US100 index, including passive holdings,
  rebalance flows and liquidity impact.

The project is presented through two concise PDF reports and a reproducible
Python notebook supported by the original constituent dataset.

## Repository Contents

### [`US INDEX CASE STUDY.pdf`](./US%20INDEX%20CASE%20STUDY.pdf)

The research component of US Indexes, focused on Linde plc and the Russell US
Indexes. It covers:

- Russell's quarterly and intra-quarter share and free-float update process;
- reconstruction of Linde's stated shares outstanding and public-filing
  free-float estimate;
- SEC filings that can help forecast future float changes;
- FTSE Russell's country-classification methodology;
- Linde's likely classifications at the 2020 and 2021 June Reconstitutions;
  and
- remaining uncertainties and the additional evidence needed to increase
confidence.

### [`US100_Modelling_Study_Report.pdf`](./US100_Modelling_Study_Report.pdf)

The presentation version of the quantitative modelling study. It summarises:

- estimated assets tracking US100;
- RTX's current index weight;
- GOOG passive ownership;
- ORCL's revised weight following a free-float increase;
- the top flows from a hypothetical AAPL deletion;
- liquidity pressure measured in average-daily-volume units; and
- the effect of a 20-for-1 AMZN stock split.

The charts in this PDF are exact high-resolution renders of the charts included
in the notebook.

### [`Model/US100_Modelling_Study_Final.ipynb`](./Model/US100_Modelling_Study_Final.ipynb)

The complete executable analytical model and audit trail. It includes:

- methodology and assumptions;
- all Python calculations;
- native pandas outputs;
- the exact model charts;
- validation assertions;
- tracking-factor sensitivity analysis; and
- final answers to all modelling questions.

### [`Model/US100_constituents (1) (1).csv`](./Model/US100_constituents%20%281%29%20%281%29.csv)

The original 100-constituent dataset used by the notebook.

## Model Architecture

The US100 model follows six stages:

1. **Load and validate the data**

   The notebook loads `US100_constituents (1) (1).csv`, checks the required
   fields and confirms that the index contains 100 constituents.

2. **Construct the free-float index**

   - Float shares = shares outstanding x free float
   - Float market capitalisation = price x float shares
   - Index weight = security float market capitalisation / total index float
     market capitalisation

3. **Estimate passive ownership**

   - Tracking AUM = total index float market capitalisation x 4.3%
   - Passive shares = security float shares x 4.3%

4. **Model index and corporate actions**

   The model calculates constituent weights and holdings, changes ORCL's free
   float, deletes AAPL without replacement and applies a 20-for-1 AMZN split.

5. **Estimate liquidity impact**

   - Flow / ADV = absolute flow shares / 30-trading-day average volume

6. **Validate the results**

   Assertions verify that weights sum to 100%, AAPL sale proceeds equal
   purchases, net flow rounds to zero and the AMZN split leaves dollar exposure
   unchanged.

### Instructions

1. Open the `Model/` folder and keep the notebook and
   `US100_constituents (1) (1).csv` together.
2. Open `Model/US100_Modelling_Study_Final.ipynb`.
3. Select a Python environment containing `pandas==2.2.3`.
4. Choose **Restart Kernel and Run All**.
5. Confirm the initial checks report:

   - `Constituents: 100`
   - `Weight check: 100.000000%`

6. Confirm that the validation section completes without an assertion error.

The submitted model runs offline because the required ADV observations are
cached inside the notebook.

## ADV Data

The liquidity analysis uses Yahoo Finance daily share volume for 30 trading
sessions from **7 May 2026 to 18 June 2026**, retrieved on **21 June 2026**.

For a live event, the included `fetch_yahoo_adv(...)` function can refresh the
data using a window ending immediately before the expected rebalance trade
date. Internet access is required only for that optional refresh.

## Methodology Note

The case-study brief describes index weight as free-float market capitalisation
divided by tracking AUM. This would not produce weights summing to 100%.
Therefore, the model uses the standard index-weight definition:

```text
Index weight = security float market capitalisation / total index float market capitalisation
```

Tracking AUM is then used to translate index weights and weight changes into
passive holdings and estimated trading flows.

## Sources

The reports and model draw primarily on:

- FTSE Russell, *Russell US Equity Indexes Construction and Methodology*;
- FTSE Russell, *Free Float Restrictions*;
- FTSE Russell, *Corporate Actions and Events Guide*;
- Linde plc SEC filings available through EDGAR;
- the Intropic Tracking Guide;
- the supplied US100 constituent dataset; and
- Yahoo Finance daily share-volume data for the stated ADV window.
