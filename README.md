# Capstone

This repository is a notebook-driven labor-policy analysis workspace focused on the early termination of pandemic unemployment benefits and the resulting employment or job-finding outcomes, with particular attention to low-wage workers.

The repo is research-useful, but it is still notebook-heavy and only partially reproducible. The files added in this pass are meant to make the current state easier to audit, present, and tighten into a thesis-ready workflow.

## Project Context

This capstone studies a 2021 U.S. labor-policy shock: the early termination of federal pandemic unemployment programs by a subset of states before the national September expiration. The project uses that staggered policy change as a quasi-experimental setting to test whether lower-wage workers responded differently than other workers.

The framing is partly motivated by Holzer-style post-policy employment analysis, but the current preferred contribution here is a triple-difference extension using CPS microdata and a low-wage versus other-wage comparison.

## Current Finding Snapshot

These are the current saved findings from the preferred notebook path, not a final paper claim:

- Preferred DDD estimate: the saved `TreatState * Post * LowWage` coefficient is `-0.0804` with `p=0.035` in [`notebooks/SignificanceHolzerStyle.ipynb`](notebooks/SignificanceHolzerStyle.ipynb).
- Other-wage subgroup DD: the saved `TreatState * Post` coefficient is `0.0472`, with borderline significance in the notebook output.
- Low-wage subgroup DD: the saved `TreatState * Post` coefficient is `-0.0240`, not statistically significant in the notebook output.
- Saved placebo result: null in 2018.
- Saved leave-one-state-out results: remain negative across the displayed state exclusions.

These findings support the current interpretation that any apparent gains from early termination were not concentrated among the low-wage subgroup the project treats as most vulnerable.

## Current Repo Status

- Primary interface: Jupyter notebooks.
- Best-supported saved DDD results: [`notebooks/SignificanceHolzerStyle.ipynb`](notebooks/SignificanceHolzerStyle.ipynb).
- Baseline TWFE reference: [`baseline_model.ipynb`](baseline_model.ipynb).
- Exploratory sensitivity stack: [`archive/notebooks/FirstValidTimeFrame.ipynb`](archive/notebooks/FirstValidTimeFrame.ipynb), [`archive/notebooks/Significant.ipynb`](archive/notebooks/Significant.ipynb), and related archived variants.

## Repo Layout

- `data/raw/`: raw policy, COVID, employment, and OxCGRT inputs kept locally in the repo, plus locally referenced CPS inputs.
- `data/processed/`: processed state-level panel files.
- `data/outputs/`: generated research-facing outputs such as extracted result snapshots.
- `notebooks/`: curated notebook copy plus exported figures.
- `archive/notebooks/`: exploratory and historical notebook variants retained for provenance, not active analysis.
- `prepare_panel_for_twfe.py`: county-level panel construction script.
- `docs/RESEARCH_READINESS.md`: suggested path from current repo state to a submission-ready workflow.
- `docs/ROBUSTNESS_PLAYBOOK.md`: prioritized robustness agenda aimed at likely reviewer criticism.
- `scripts/extract_saved_results.py`: extracts saved notebook outputs into a stable Markdown snapshot.

## Environment Setup

Python and package versions were not pinned in the repo before this pass. The new `requirements.txt` reflects packages imported by the committed notebooks, but the versions are intentionally unpinned until you validate a final environment.

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

## Data Inputs

- Local input copies exist in `data/raw/`, including policy, COVID, employment, and a policy milestones file used by the active notebook path.
- The preferred notebook also expects a large CPS extract named `cps_00006.csv`, which is intentionally not tracked in normal Git history.
- Several archived notebooks still expect files under `~/Downloads/`.
- Some archived notebook variants reference CPS extracts not present here, including `cps_00004.csv` and `cps_00005.csv`.
- External CPS link retained from the earlier repo state:
  [Google Drive CPS file](https://drive.google.com/file/d/1QQ9AD1ozgE56n1smAfVc4Vl-ml1inrUf/view?usp=sharing)

For research-ready replication, the next cleanup step is to normalize every notebook and script to repo-relative paths.

## Recommended Workflow

1. Review [`docs/RESEARCH_READINESS.md`](docs/RESEARCH_READINESS.md) to see which notebooks are closest to a defensible analysis path.
2. Review [`docs/ROBUSTNESS_PLAYBOOK.md`](docs/ROBUSTNESS_PLAYBOOK.md) before presenting or writing up results.
3. Use [`notebooks/SignificanceHolzerStyle.ipynb`](notebooks/SignificanceHolzerStyle.ipynb) as the current best-supported source for the main DDD, placebo, and leave-one-state-out results.
4. Use [`baseline_model.ipynb`](baseline_model.ipynb) for baseline TWFE context.
5. Treat notebooks under [`archive/notebooks/README.md`](archive/notebooks/README.md) as exploratory history, not the primary analysis path.
6. Generate a stable text snapshot of saved notebook outputs:

```bash
python3 scripts/extract_saved_results.py --output data/outputs/research_snapshot.md
```

## Panel Construction

The repo includes one committed prep script:

```bash
python3 prepare_panel_for_twfe.py
```

That script expects county employment, policy, and COVID CSVs in `~/Downloads` and writes `twfe_panel_county_data.csv` to the repo root.

## What Still Needs To Happen

- Freeze one canonical analysis path and archive the rest as exploratory.
- Move repeated modeling logic out of notebooks into scripts or a small module.
- Normalize all file paths away from `~/Downloads`.
- Export final tables and figures from the preferred specification into tracked outputs.
- Add a variable dictionary and treatment-definition memo.
- Pin package versions once the final notebook or script is locked.
