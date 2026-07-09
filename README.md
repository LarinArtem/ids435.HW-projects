# IDS 435 — Optimization & Operations Research Projects

Coursework in mathematical optimization, built around **Gurobi** in Python. The repository moves from classic linear/integer programming exercises to two substantial applied projects: political districting (gerrymandering) as an integer program and mean–variance / Black–Litterman portfolio optimization on real market data.

## Repository contents

| Notebook | Topic |
|---|---|
| `hw3.ipynb` | Inventory/ordering optimization — multi-period order-quantity models with case analysis (when it is optimal to order at capacity, below capacity, or nothing) |
| `hw5.ipynb` | LP/MIP modeling exercises, including two alternative formulations of the same problem shown to reach the same optimum |
| `gerrymander.ipynb` | Electoral districting as binary integer programming |
| `multi_tool_v4.ipynb` | Portfolio optimization with real price data, efficient frontier, and Black–Litterman views |

## Project 1 — Gerrymandering as Integer Programming (`gerrymander.ipynb`)

Models district design as a constrained assignment problem:

- **Decision variables** — binary `x[p, d]` assigning each precinct `p` to exactly one district `d`.
- **Constraints** — every precinct assigned once; district populations balanced within tolerance; geographic structure respected so districts are sensible contiguous shapes.
- **Objective** — an optimization criterion over the party-vote composition of districts, which makes the political trade-offs of districting explicit: the same machinery that draws "fair" maps can maximize seats for one side, which is exactly the point the exercise illustrates.
- **Visualization** — helper functions generate random test instances and plot the optimized district maps with Matplotlib, so the effect of each constraint is visible on an actual map.

## Project 2 — Portfolio Optimization (`multi_tool_v4.ipynb`)

An applied quantitative-finance pipeline on a 33-stock universe (ADBE, GOOGL, MSFT, XOM, PFE, …):

1. **Data** — three years of daily prices (2022–2025) pulled from the **EODHD API**, transformed into returns, expected returns, and a covariance matrix with Pandas/NumPy.
2. **Mean–variance optimization** — a quadratic program solved with Gurobi, with realistic constraints (full investment, position limits / sector exposure).
3. **Efficient frontier** — the frontier is traced by re-solving across target returns and plotted (risk on x, return on y) with individual stocks overlaid, making the diversification benefit visual.
4. **Black–Litterman model** — `build_black_litterman_posterior()` implements the full posterior: market-implied equilibrium returns `π = δΣw_mkt`, investor views encoded in `(P, Q, Ω)`, and the closed-form posterior mean and covariance. This blends market equilibrium with subjective views instead of relying on noisy historical mean returns.

## Homework highlights

- `hw3.ipynb` works through newsvendor-style ordering logic with written interpretation of each optimality case (interior optimum, capacity-constrained, zero order), not just solver output.
- `hw5.ipynb` demonstrates model-formulation skill by solving one problem two different ways and verifying identical optima.

## Tech stack

Python, `gurobipy` (LP/MIP/QP solver), `pandas`, `numpy`, `matplotlib`, `requests` + EODHD API.

## Running the notebooks

1. Install Gurobi and a license (free academic licenses available at gurobi.com).
2. ```bash
   pip install gurobipy pandas numpy matplotlib requests
   ```
3. For `multi_tool_v4.ipynb`, insert your EODHD API key in the data-download cell.
4. Run cells top to bottom; the gerrymandering notebook includes its own instance generator, so no external data is needed.
