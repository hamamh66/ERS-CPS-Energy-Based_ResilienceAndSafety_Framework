# ERS-CPS: Energy-Based Early Warning and Resilience Analysis for Cyber-Physical Infrastructure

Open-source, fully reproducible implementation of a risk-aware visual analytics framework that integrates heterogeneous monitoring information into a node-level risk-energy state for early warning in industrial cyber-physical systems.

For each node of an infrastructure graph, communication quality (packet loss, latency), integrity and trust events, system stress, and graph-based neighbor pressure are mapped onto one bounded, dimensionless **risk energy** with an explicit memory state. From this state the framework derives:

- analytical stability coordinates (growth descriptor and stability margin);
- a six-term **Critical Transition Indicator (CTI)** built from positive energy velocity and curvature, normalized memory and pressure, a risk increment, and an energy-entropy term;
- a one-step residual forecast of the next energy value;
- fused risk scores and a retrospective graph-based node ranking.

## What the notebook reproduces

`notebooks/ERS_CPS_reproduction.ipynb` runs end to end on a CPU in a few minutes and regenerates every figure and table in `results/`:

| Analysis | Main outputs |
|---|---|
| Seed-controlled simulation: Barabási–Albert graph, 18 nodes, *m* = 2, seed 42, 450 steps in nominal / degraded / adversarial regimes | `node_attributes.csv`, `scenario_*_summary_v17.csv`, figures 1–2 |
| CTI first-warning lead times at the 90th-percentile training threshold | `lead_times_v17.csv`, `lead_time_summary_v17.csv`, figure 3 |
| Fused risk scores (descriptive, full horizon) | `final_method_comparison_v17.csv`, figure 5 |
| One-step energy forecast and persistence baseline (late adversarial window) | `scenario_regression_evaluation_v17.csv`, `persistence_baseline.csv`, figure 6 |
| Retrospective node ranking | `worst_nodes_v17.csv`, figure 4 |
| Sensitivity to the threshold quantile and to ±20 % perturbations of the six CTI weights (200 draws) | `sensitivity_*.csv`, figure 7 |
| Comparison with single CTI terms and classical critical-slowing-down statistics (rolling variance, AC(1), skewness, variance + AC(1)) at a 1 % nominal alarm calibration | `csd_comparison.csv`, figure 8 |
| Operator-preference sweep of warning time against nominal false alarms and pre-failure alarm burden | `tradeoff_sweep.csv`, `tradeoff_profiles.csv`, figure 9 |

`results/others/results_summary.json` collects the headline numbers and `results/others/environment.json` records the package versions used.

### Headline results (seed 42)

- At the 90th-percentile threshold (τ = 0.1406) all 18 failing nodes are warned before their first failure; mean / median / maximum lead time 74.9 / 65.5 / 118 steps.
- At thresholds calibrated to a 1 % nominal alarm rate, the CTI warns all 18 nodes with a mean lead time of 156.2 steps and a ROC-AUC of 0.947 for failure within 10 steps. The best classical statistic, rolling variance, reaches 115.7 steps and 0.861. The memory term alone discriminates better (0.974) but warns later (111.1 steps).
- Under 200 random ±20 % perturbations of the CTI weights, all nodes remain warned, the mean lead time stays between 63.6 and 88.6 steps, and the mean-CTI node ranking keeps Spearman ρ ≥ 0.983.
- The one-step forecast in the adversarial test window has MAE 0.1035, RMSE 0.1151, R² = −0.398 (persistence: 0.0916, 0.1287, −0.747).

## Scope

All operational quantities (trust, packet loss, latency, stress, failures) are **simulated**, and failure labels share their generating mechanisms with the indicator inputs. Lead times, discrimination scores and rankings are retrospective characterizations of this simulation, not prospective performance estimates. NASA CMR granule metadata can optionally be used as a timeline scaffold (`USE_NASA_CMR = True`); it carries no operational information, and the reported results use the synthetic fallback (default).

## Running

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute notebooks/ERS_CPS_reproduction.ipynb --output ERS_CPS_reproduction.ipynb
```

or open the notebook in Google Colab and run all cells. Outputs are written to `outputs/` next to the notebook; in Colab the small result files are also copied to `MyDrive/Outputs/ERS-CPS/`.

The results in `results/` were generated with Python 3.13.18, NumPy 2.5.3, pandas 3.0.5, scikit-learn 1.9.1, NetworkX 3.7, SciPy 1.18.1 and Matplotlib 3.11.2.

## Repository layout

```
notebooks/ERS_CPS_reproduction.ipynb   executed reproduction notebook
results/figures/                       figures 1–9
results/tables/                        all CSV result tables
results/others/                        results_summary.json, environment.json
requirements.txt
LICENSE                                MIT
```

## License

MIT, see `LICENSE`.
