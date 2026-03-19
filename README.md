# CCGT Energy Optimization

MILP-based optimization for Combined Cycle Gas Turbine (CCGT) economic dispatch in wholesale electricity markets. This project implements two modeling approaches used in different ISO markets—CAISO (configuration-based) and PJM (pseudo-unit)—demonstrating production-grade optimization techniques for energy analytics.

## Overview

The project optimizes profit-maximizing dispatch over a 168-hour horizon (March 21–27, 2022) using hourly NP15 electricity prices and natural gas costs. Two notebooks implement distinct market representations:

### CAISO Representation
Single CCGT modeled with **five operating configurations** (OFF, 1 CT, 2 CT, 1x1, 2x1). The model selects exactly one configuration per hour, with configuration-specific min/max MW, heat rates, VOM, and startup costs. This reflects how CAISO and other western markets often model combined-cycle units.

### PJM Representation
The same physical CCGT modeled as **two independent pseudo-units** (Base Unit + Incremental Unit), each optimized separately. Unit 1 (57–190 MW) represents the gas turbine; Unit 2 (0–420 MW) represents the steam turbine/incremental capacity. This mirrors PJM’s pseudo-unit bidding structure.

## Project Structure

| File | Description |
|------|-------------|
| `CCGT_with_CAISO_representation.ipynb` | Single-unit MILP with config switching; outputs `CCGT_CAISO.csv` |
| `CCGT_with_PJM_representation.ipynb` | Two pseudo-units, separate optimizations; outputs `CCGT_PSEUDO.csv` |
| `Prices.xlsx` | Input: hourly electricity prices (NP15) |
| `CCGT_CAISO.csv` | Output: CAISO dispatch schedule |
| `CCGT_PSEUDO.csv` | Output: PJM-style pseudo-unit dispatch |

## Requirements

- **Python 3.8+**
- **Gurobi Optimizer** — Required to run the MILP models

### Gurobi License

A valid Gurobi license is required. Gurobi offers free academic licenses for students and researchers.

**Setup:**
1. Install: `pip install gurobipy`
2. Obtain a license from [Gurobi](https://www.gurobi.com/downloads/end-user-license-agreement-academic/)
3. Place `gurobi.lic` in the project root
4. Notebooks set `GRB_LICENSE_FILE` automatically

> The `gurobi.lic` file is in `.gitignore` and is not committed. You must add your own license to run the code.

## Installation

```bash
pip install pandas numpy matplotlib openpyxl gurobipy
```

## Usage

1. Place `Prices.xlsx` in the project directory (columns: `OPERATING_DATE`, `HOUR_ENDING`, `NP15 ($/MWh)`)
2. Place your `gurobi.lic` file in the project root
3. Run the desired notebook:
   - **CAISO:** `CCGT_with_CAISO_representation.ipynb` — run cells for data load, MILP, outputs
   - **PJM:** `CCGT_with_PJM_representation.ipynb` — run cells for bid curve, data load, MILP, outputs

## Outputs

**CAISO notebook:**
- `CCGT_CAISO.csv` — Hourly schedule (date, hour, price, configuration, MW)
- Plots: MW and configuration vs time
- Metrics: Revenue, costs, fuel costs, starts, capacity factor, gross margin

**PJM notebook:**
- `CCGT_PSEUDO.csv` — Hourly schedule (date, hour, price, gas price, MW_Unit1, MW_Unit2)
- Plot: MW vs time for both units
- Metrics: Per-unit revenue, costs, fuel costs, starts, capacity factor, gross margin

## License

Educational and portfolio use. Gurobi usage is subject to Gurobi’s license terms.
