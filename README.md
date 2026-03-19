# CCGT Energy Optimization

A Mixed-Integer Linear Programming (MILP) model for optimizing the economic dispatch of a Combined Cycle Gas Turbine (CCGT) power plant in wholesale electricity markets. This project demonstrates production-grade optimization techniques applied to energy analytics—a core competency for ML and analytics roles in power systems and utilities.

## Overview

This optimization determines the profit-maximizing operating schedule for a CCGT over a 168-hour horizon (one week), given hourly electricity prices and natural gas costs. The model selects among five operating configurations (OFF, 1 CT, 2 CT, 1x1, 2x1), schedules generation levels, and accounts for startup costs and variable operating expenses.

**Key capabilities:**
- Multi-configuration unit commitment with realistic operational constraints
- Startup logic with proper binary variable formulation
- Revenue maximization under price uncertainty
- Automated output generation (CSV, visualizations, summary metrics)

## Technical Approach

The model is formulated as a MILP and solved using [Gurobi](https://www.gurobi.com/), an industry-standard commercial solver for large-scale optimization. Decision variables include:

- **u[c,t]** — Binary: configuration `c` active in hour `t`
- **p[c,t]** — Continuous: MW generation from configuration `c` in hour `t`
- **v[c,t]** — Binary: startup event for configuration `c` in hour `t`

Constraints enforce single-configuration operation, generation bounds, and correct startup detection. The objective maximizes profit (revenue minus variable and startup costs).

## Requirements

- **Python 3.8+**
- **Gurobi Optimizer** (commercial solver)
- **Gurobi license** — Required to run the optimization

### Gurobi License

This project requires a valid Gurobi license. Gurobi offers:

- **Academic license** — Free for students, faculty, and researchers at degree-granting institutions
- **Commercial license** — For industry use

**To run this code:**
1. Install Gurobi: `pip install gurobipy`
2. Obtain a license from [Gurobi's license center](https://www.gurobi.com/downloads/end-user-license-agreement-academic/)
3. Place your `gurobi.lic` file in the project root directory
4. The notebook configures the license path automatically via `GRB_LICENSE_FILE`

> **Note:** The `gurobi.lic` file is excluded from version control (see `.gitignore`) for security and licensing compliance. Users must obtain and place their own license to run the optimization.

## Installation

```bash
pip install pandas numpy matplotlib openpyxl gurobipy
```

## Usage

1. Ensure `Prices.xlsx` is in the project directory (hourly electricity prices with columns: `OPERATING_DATE`, `HOUR_ENDING`, `NP15 ($/MWh)`)
2. Place your `gurobi.lic` file in the project root
3. Open `HW2.ipynb` and run all cells in order

## Outputs

The model produces:

- **CCGT_CAISO.csv** — Hourly dispatch schedule (date, hour, price, configuration, MW)
- **Plots** — MW generation and configuration over time
- **Summary metrics** — Total revenue, costs, fuel costs, number of starts, capacity factor, gross margin ($/kW)

## Project Structure

```
.
├── README.md
├── HW2.ipynb          # Main notebook: data loading, MILP, outputs
├── Prices.xlsx        # Input: hourly electricity prices
├── CCGT_CAISO.csv     # Output: optimized dispatch schedule
└── .gitignore
```

## License

This project is for educational and portfolio purposes. Gurobi usage is subject to Gurobi's license terms.
