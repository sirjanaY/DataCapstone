# Robustness Playbook

This file is the shortest path to making the current project more defensible against predictable criticism.

## External Benchmark: The Holzer Paper

The reference study is:

- Harry J. Holzer, Glenn Hubbard, and Michael R. Strain.
  `Did pandemic unemployment benefits increase unemployment? Evidence from early state-level expirations.`
  *Economic Inquiry* 62(1), 2024, 24-38.
  DOI: `10.1111/ecin.13180`
- Earlier working-paper version:
  NBER Working Paper 29575, `Did Pandemic Unemployment Benefits Reduce Employment? Evidence from Early State-Level Expirations in June 2021.`

### What Holzer et al. claim

At a high level, the paper studies early June 2021 state exits from expanded UI programs and reports that unemployed-to-employed transitions rose after early exit, especially among prime-age workers.

### Why that matters for this repo

Your final notebook is best framed as an extension or challenge to that literature, not yet as a drop-in replication of Holzer.

This repo currently differs from Holzer in at least some of the following ways:

- treatment-state definition appears to come from the repo policy file and staggered state exits
- subgroup focus is low-wage versus other-wage rather than only Holzer's published subgroup framing
- notebook variants use different sample years, placebo years, and control sets
- some variants rely on repo-local saved outputs rather than a single scripted pipeline

That makes robustness work especially important.

## Core Reviewer Questions And The Best Countermoves

### 1. "You are not really replicating Holzer."

This is the first criticism to expect.

Best response:

1. Build an exact Holzer baseline.
2. Then present your low-wage DDD as a heterogeneity extension.

Minimum exact-replication items to lock down:

- treatment group: June 2021 early exits only, matching Holzer's definition
- separate treatment coding for states exiting both FPUC and PUA versus FPUC-only states if Holzer separates them
- sample restriction: prime-age workers if that is the paper's headline design
- outcome: unemployed-to-employed transition definition exactly matched
- controls: COVID and policy controls matched to the published spec
- event-study window and reference period matched exactly

If you do not do this, critics can always say your negative low-wage DDD is just a different design rather than a challenge to Holzer.

### 2. "Your DDD passes PTA, but what about the underlying DDs?"

This is a real vulnerability.

The DDD event-study test checks whether the *difference between subgroup DDs* is flat pre-treatment. It does **not** prove that each subgroup DD satisfies parallel trends on its own.

Best response:

Run and export three separate pretrend diagnostics for the canonical spec:

- DDD event study
- low-wage subgroup DD event study
- other-wage subgroup DD event study

That way you can say exactly which identifying assumptions are supported.

### 3. "Low-wage is ad hoc."

This is another major line of attack.

Best response:

Pre-register or pre-announce a short family of low-wage definitions and show the sign pattern is stable across them.

Recommended sequence:

1. Current industry-based low-wage definition in the final notebook.
2. Holzer-style subgroup proxy if relevant to the paper you are extending.
3. Education-based proxy such as no-college.
4. Age-based proxy if theory supports weaker labor-market attachment.
5. Alternative service-sector-only subgroup such as leisure/hospitality and retail.

Do not test ten versions and cherry-pick. Pick three to five with a clear rationale.

### 4. "Your treatment timing is noisy."

If treatment timing is off, the DDD can be biased.

Best response:

- use exact state-specific policy dates from the policy file
- test a June-only treatment sample
- test a sample that drops ambiguous or staggered late-exit states
- test a version aligned to Holzer's published treatment grouping

If the sign flips when timing is cleaned up, that is a serious warning.

### 5. "This is driven by a few states."

You already have the start of an answer with leave-one-state-out.

Best response:

- keep the current LOO table and figure
- add leave-one-region-out if treated states cluster geographically
- add wild cluster bootstrap p-values by state

State-cluster inference is especially important because the number of treated clusters is not huge.

### 6. "COVID or reopening effects are doing the work."

This is one of the most plausible confounding stories.

Best response:

Test nested models:

1. no controls
2. COVID cases only
3. COVID plus stringency
4. state-specific linear trends
5. optionally vaccination or hospitalization controls if available and defensible

If your sign is only present in one narrow control set, critics will attack that immediately.

### 7. "Your result is sample-fragile."

Best response:

Run the same canonical model on:

- prime-age only
- all ages
- no-college only
- no-college within low-wage sectors if that is theoretically important
- states with sufficient pre-period observations only

You do not need every possible sample. You need a small, theory-driven grid.

### 8. "Your result only exists because of functional-form choices."

Best response:

Compare:

- linear probability model
- logit or probit for the same transition outcome
- weighted versus unweighted versions if weights matter

The coefficient scale will differ, but the sign and inference pattern should not collapse.

### 9. "Your placebo design is inconsistent."

The repo currently uses different placebo years across notebook variants.

Best response:

Choose one placebo strategy and stick to it:

- calendar placebo year, matched to the same months
- fake cutoff moved earlier in the same year
- placebo subgroup such as college graduates if theory says they should respond differently

Ideally keep at least one time placebo and one subgroup placebo.

### 10. "You are overinterpreting relative disadvantage as absolute harm."

This is mostly a framing issue.

Best response:

Be precise:

- DDD identifies relative treatment effects across groups.
- A negative DDD means low-wage workers did worse relative to the comparison group.
- It does not, by itself, prove who received benefits or that low-wage workers were absolutely worse off in every specification.

That precision makes the paper stronger, not weaker.

## Priority Order

If you only do a handful of things before presenting or writing, do these in order:

1. Exact Holzer-style baseline replication.
2. Canonical low-wage DDD with subgroup-specific DD event studies.
3. Wild cluster bootstrap plus leave-one-state-out.
4. Alternative low-wage definitions with a predeclared shortlist.
5. Treatment-timing sensitivity.
6. Control-set sensitivity.

## Deliverables To Export

For the final paper-ready version, export these as tracked outputs:

- one coefficient table for the canonical Holzer-style baseline
- one coefficient table for the preferred low-wage DDD
- one pretrend figure each for DDD, low-wage DD, and other-wage DD
- one placebo table
- one robustness table with alternative low-wage definitions
- one inference table with standard clustered SEs and wild bootstrap p-values

If these are all in `data/outputs/`, critics will have a much harder time claiming the result depends on hidden notebook paths.
