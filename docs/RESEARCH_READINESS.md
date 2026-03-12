# Research Readiness

This repo already contains useful analysis, but it is not yet in a state where another researcher could reproduce the full argument cleanly from a single entrypoint.

## What Is Already Here

- Raw and processed data files are committed locally under `data/raw/` and `data/processed/`.
- The repo contains saved notebook outputs for baseline TWFE, DDD, placebo, and leave-one-state-out checks.
- Exported figures already exist under `notebooks/`.
- `prepare_panel_for_twfe.py` shows one concrete data-prep step as a script rather than only inside a notebook.

## Main Gaps

### 1. No single source of truth

Multiple notebooks cover overlapping questions:

- `notebooks/SignificanceHolzerStyle.ipynb`: strongest saved DDD, placebo, and LOO results. Best current candidate for a primary notebook.
- `baseline_model.ipynb`: baseline TWFE reference.
- `archive/notebooks/SignificanceHolzerStyle.ipynb`: root-level variant with overlapping logic.
- `archive/notebooks/FirstValidTimeFrame.ipynb`: large sensitivity and iteration notebook with repeated code paths.
- `archive/notebooks/Significant.ipynb` and `archive/notebooks/SignificantBADTIMEFRAME.ipynb`: broad exploratory robustness notebooks.
- `archive/notebooks/Untitled-2.ipynb`: scratch and intermediate work.
- `archive/notebooks/BAD_BASELINE.ipynb`: saved poor-fit baseline result.

For a thesis or paper, one notebook or script chain needs to be declared canonical, and the rest should be labeled exploratory or archived.

### 2. Path handling is inconsistent

Several notebooks read data from `~/Downloads`, while the repo also contains local copies under `data/raw/`. A clean replication path requires one config rule:

- either use repo-relative data paths everywhere
- or use a documented external-data directory with one config file

### 3. Environment is not pinned

The repo previously had no environment specification. `requirements.txt` now documents imported packages, but a final paper-ready version should pin exact versions after validation.

### 4. Findings live inside notebook JSON

Saved outputs were not previously extracted into a stable review artifact. The new script `scripts/extract_saved_results.py` generates `data/outputs/research_snapshot.md` so current findings can be reviewed without opening notebooks.

### 5. Shared modeling logic is duplicated

Functions such as event-study coefficient extraction, plotting, and DDD setup appear across notebooks in slightly different forms. That makes it hard to know which version produced the result you plan to defend.

## Best Current Presentation Path

If you had to present from the repo today, the strongest path is:

1. Main result:
   `notebooks/SignificanceHolzerStyle.ipynb`
2. Baseline context:
   `baseline_model.ipynb`
3. Sensitivity or contrast:
   `archive/notebooks/FirstValidTimeFrame.ipynb`
4. Stable text summary:
   `data/outputs/research_snapshot.md`

That path is good enough for a serious presentation, but not yet ideal for full replication.

## Highest-Value Next Steps

### Short term

1. Freeze a canonical notebook:
   Pick one notebook that defines the final treatment states, low-wage definition, controls, event-study validation, and placebo logic.
2. Normalize data paths:
   Update the canonical notebook and prep script to use `data/raw/` first, not `~/Downloads`.
3. Export final tables:
   Write the final coefficient tables and sample sizes into `data/outputs/` as CSV or Markdown.
4. Add a methods memo:
   Define `TreatState`, `Post`, `LowWage`, sample restrictions, and placebo year in one file.

### Medium term

1. Move shared code into `scripts/`:
   Put panel prep, model estimation, and event-study extraction into reusable Python files.
2. Add a reproducible runner:
   One command should regenerate the preferred panel, key tables, and key figures.
3. Pin versions:
   Lock the package environment once the canonical pipeline is stable.

### Submission ready

1. Keep scratch notebooks archived under `archive/notebooks/` and remove them from the active workflow.
2. Add a data dictionary for raw and processed files.
3. Add a replication checklist that states exactly which outputs support the final argument.
