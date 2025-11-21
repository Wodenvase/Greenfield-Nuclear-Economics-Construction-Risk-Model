# AtomicBuild: Greenfield Nuclear Economics & Construction Risk Model

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Model](https://img.shields.io/badge/Model-Greenfield%20Project%20Finance-green)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![License](https://img.shields.io/badge/License-Old%20Economy%20(Private)-red)

**AtomicBuild** is a stochastic Discounted Cash Flow (DCF) model designed to simulate the investment viability of new nuclear reactor construction (Greenfield projects).

While the previous **AtomicVal** project focused on the value of existing assets, **AtomicBuild** quantifies the risks of creating new ones. It utilizes historical data from the IAEA PRIS database to model construction delays ("Construction Drag") and capital expenditure curves, providing a data-driven assessment of the "Valley of Death" inherent in large infrastructure financing.

> **RESTRICTED ACCESS:** This project is protected under the **Old Economy License**. It is intended for proprietary internal use only.

---

## The Investment Thesis

New nuclear construction is characterized by a binary risk profile: projects either generate massive long-term cash flows or destroy capital through prolonged delays and cost overruns. This model replaces brochure estimates with historical reality.

**Key Capabilities:**
* **Historical Benchmarking:** Construction timelines are derived from actual start-to-grid connection data from the IAEA, segmenting performance by era and region.
* **S-Curve CAPEX Modeling:** Implements realistic "Sigmoid" spending curves rather than flat-line averages, accurately modeling the peak funding burdens during the mid-construction phase.
* **Stochastic Risk Assessment:** Uses Monte Carlo simulations (10,000 iterations) to quantify the probability of positive NPV under uncertain cost and schedule conditions.
* **Investment Committee Logic:** An automated dashboard that accepts or rejects projects based on specific Risk/Reward hurdles (e.g., "80% confidence of >10% IRR").

---

## Model Architecture

The project is structured as a sequential data pipeline across 7 Jupyter Notebooks.

### 1. Construction Drag Analysis (01_construction_drag.ipynb)
* **Input:** Raw IAEA PRIS Database.
* **Logic:** Calculates actual construction duration (Grid Connection Date - Construction Start Date). Segments data by Era (Pre-1980, 1980-2000, Post-2000) to determine if industry learning curves are improving or degrading.
* **Output:** `construction_durations.csv` (Probability Distributions).

### 2. CAPEX S-Curve Modeling (02_capex_schedule.ipynb)
* **Logic:** Defines the "Model Plant" (1100 MW). Applies a cumulative normal distribution to spread Overnight Capital Costs over the construction timeline, creating a realistic cash outflow schedule.
* **Scenarios:** "The Pitch" (5 years), "The Reality" (8 years), "The Disaster" (12 years).
* **Output:** `capex_schedule.csv`.

### 3. Operational Economics (03_operational_economics.ipynb)
* **Logic:** Models the post-construction cash inflows. Assumes 60-year operational life, 92% capacity factor, and industry-standard O&M costs.
* **Output:** `operational_economics.csv`.

### 4. The Greenfield DCF Engine (04_greenfield_dcf.ipynb)
* **Logic:** Combines CAPEX outflows and Operational inflows into a unified timeline. Calculates NPV, IRR, and Payback Period.
* **Visualization:** The "J-Curve" (Valley of Death) showing the depth and duration of the cumulative cash deficit.
* **Output:** `greenfield_results.csv`.

### 5. Sensitivity Analysis (05_sensitivity_matrix.ipynb)
* **Logic:** Generates 2D Heatmaps mapping Project NPV against two critical variables: Overnight Cost ($/kW) and Electricity Price ($/MWh).
* **Key Insight:** Identifies the "Breakeven Frontier"—the maximum allowable construction cost for a given market power price.

### 6. Monte Carlo Simulation (06_monte_carlo.ipynb)
* **Logic:** Vectorized simulation of 10,000 project lifecycles.
* **Inputs:** Randomizes Construction Duration (Lognormal), Cost (Triangular), and Power Price (Normal) based on historical parameters.
* **Output:** Probability distributions for NPV and ROI.

### 7. Investment Committee Dashboard (07_investment_committee.ipynb)
* **Tech:** `ipywidgets` (Interactive) or Static Analytics.
* **Features:** Allows users to set specific risk tolerances (e.g., "Must have 90% probability of breakeven"). Visualizes the "Safe Harbor" of viable project parameters.

---

## Key Assumptions

| Parameter | The Pitch (Low) | The Reality (Med) | The Disaster (High) |
| :--- | :--- | :--- | :--- |
| **Overnight Cost** | $4,000 / kW | $7,000 / kW | $11,000 / kW |
| **Construction Duration** | 5 Years | 8 Years | 12 Years |
| **Electricity Price** | $100 / MWh | $75 / MWh | $50 / MWh |
| **WACC** | 8.0% | 8.0% | 8.0% |

---

## Installation & Usage

1.  **Environment Setup:**
    ```bash
    git clone [https://github.com/yourusername/AtomicBuild-Greenfield.git](https://github.com/yourusername/AtomicBuild-Greenfield.git)
    cd AtomicBuild-Greenfield
    pip install pandas numpy matplotlib seaborn scipy ipywidgets
    ```

2.  **Data Prerequisite:**
    Ensure `Reactor Database - master(pris.iaea.xlsx - in.csv` is in the root directory.

3.  **Execution:**
    Run the notebooks in sequential order (01 -> 07).

---

## Visualization Previews

* **The J-Curve:** A cumulative cash flow line chart illustrating the deep capital hole created during construction and the long timeframe required to reach breakeven.
* **The Kill Zone:** A scatter plot from the Monte Carlo simulation showing the relationship between construction duration and cost, highlighting the narrow window of project viability.
* **Profitability Heatmaps:** A matrix showing NPV sensitivity to Overnight Cost and Power Price.

---

## Disclaimer & License

**Disclaimer:** This project is a simulation based on historical engineering data. It does not constitute financial advice, investment recommendations, or a solicitation to buy/sell any securities or finance any specific infrastructure projects.

**License: OLD ECONOMY LICENSE (Private)**
* **Strictly Confidential.**
* **No Public Distribution.**
* **Internal Use Only.**

Copyright (c) 2025. All Rights Reserved.
