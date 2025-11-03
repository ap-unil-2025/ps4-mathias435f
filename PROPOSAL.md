# Project Title & Category

**Portfolio Risk Analyzer — CLI VaR & ES Toolkit**
*Category:* Business & Finance Tools (Risk Management) + Data Analysis & Visualization

# Problem Statement / Motivation

Retail and student investors often evaluate portfolios using return metrics (CAGR, Sharpe) but lack an auditable, reproducible way to quantify *downside risk* across methods and horizons. This project proposes a command-line tool that computes **Value-at-Risk (VaR)** and **Expected Shortfall (ES)** for single assets and multi-asset portfolios, enabling consistent, transparent risk reporting that could be used in coursework, interviews, or research replications.

# Planned Approach & Technologies

* **Data ingestion:** CSV/Excel/JSON files and optional live download (e.g., `yfinance`) for daily prices; robust parsing and validation with `pandas`.
* **Preprocessing:** log/linear returns; missing-data handling; portfolio aggregation using user-supplied weights.
* **Risk models implemented:**

  1. **Historical Simulation** (empirical quantiles),
  2. **Parametric Normal** (μ, σ; Cornish-Fisher optional),
  3. **Parametric t-distribution** (ν estimated),
  4. **Monte Carlo** (Gaussian/t draws),
  5. **Filtered Historical Simulation** (volatility scaling via rolling EWMA/GARCH-like proxy).
* **CLI design:** `Typer` (or `Click`) with subcommands (`var`, `es`, `report`); options for confidence level (e.g., 95/99%), horizon scaling, portfolio weights, and output format.
* **Outputs:** JSON/CSV/table to stdout or file; optional PDF/HTML summary (later).
* **Engineering:** Python 3.10+, `numpy`, `scipy`, `pandas`, `Typer`, `rich` (tables/progress), `pytest` for unit tests, `mypy` type hints, `flake8/black`. Config via `YAML/TOML`. Logging with `logging`.
* **Repo structure:** `src/`, `tests/`, `docs/`, `examples/`, `data/`, `results/`, `PROPOSAL.md`, `AI_USAGE.md`.

# Expected Challenges & Mitigations

* **Numerical accuracy:** Use vectorized quantiles, explicit definitions for one-sided VaR/ES, reproducible RNG seeds; cross-check small test cases analytically.
* **Small samples / non-normal tails:** Provide t-parametric and historical/filtered methods; surface confidence intervals via bootstrap (stretch).
* **Usability:** Clear CLI help, sensible defaults, schema-validated configs, instructive error messages; include example datasets and ready-to-run commands.
* **Performance on larger datasets:** Vectorization, lazy loading, and optional down-sampling; pre-computed rolling stats.

# Success Criteria (Measurable)

* Implements **≥4** VaR methods **and** ES for each.
* Handles **single asset and portfolio** inputs with weights, producing consistent VaR/ES at user-chosen α (e.g., 1%, 5%) and horizons.
* **CLI UX:** discoverable help, non-zero exit codes on failure, JSON/CSV/table outputs.
* **Quality:** ≥70% test coverage (`pytest`), typed public APIs, `flake8/black` clean, comprehensive `README` with examples.
* **Reproducibility:** deterministic outputs given a fixed seed/config.

# Stretch Goals (If Time Permits)

* **Backtesting of VaR breaches** (Kupiec/Christoffersen).
* **EWMA/GARCH volatility scaling** for FHS.
* **PDF/HTML report generator** with plots (drawdowns, exceedances).
* **Scenario analysis** (shock correlations/volatilities).

---
