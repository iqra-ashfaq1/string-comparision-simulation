[README.md](https://github.com/user-attachments/files/26277177/README.md)
# DNA Sequence Comparison simulation

This repository contains a Jupyter notebook , which runs a small simulation study on DNA-like sequences to compare different sequence comparison methods as evolutionary divergence increases.

## What’s in the notebook

### 1) Setup and simulation parameters
- Imports (`numpy`, `pandas`, `matplotlib`) and RNG seeding for reproducibility.
- Defines key parameters:
  - `BASE_LEN = 150` (base sequence length)
  - `MOTIF_LEN = 7` (embedded motif length)
  - `DIVERGENCE_LEVELS = [0.01, 0.05, 0.10, 0.20]`
  - `N_REPS = 30` Monte Carlo replicates per divergence level
  - Indel settings for the indel-enabled mutation model: `P_INDEL = 0.20`, `INDEL_MAX_LEN = 3`

### 2) Sequence initialization
- Generates a random DNA base sequence over `{A, C, G, T}`.
- Generates a random motif and embeds it at a random position inside the base sequence.

### 3) Mutation models
Creates two mutated versions of the base-with-motif sequence for each replicate and divergence level:
- **Substitutions only**: approximately `divergence * BASE_LEN` positions are substituted.
- **Substitutions + indels**: a sequence of mutation *events* is applied; each event is a substitution or an insertion/deletion (with probability `P_INDEL`). This mode can change sequence length.

### 4) Task A — Distance metrics vs divergence
Computes and summarizes (mean ± SD across replicates):
- **Edit distance** (Levenshtein-style DP)
- **Hamming distance** (only defined when lengths match; becomes `NaN` when indels change length)

Outputs:
- A plot comparing Edit vs Hamming distance across divergence levels for both mutation modes.

### 5) Task B — Alignment scoring vs divergence
Implements two classical alignment scoring schemes (with default scores `match=1`, `mismatch=-1`, `gap=-2`) and evaluates them against the base sequence:
- **Needleman–Wunsch** (global alignment score)
- **Smith–Waterman** (local alignment score)

Outputs:
- A plot of mean alignment scores (± SD) across divergence levels for both mutation modes.

### 6) Task C — PWM motif scanning vs divergence
Builds a simple PWM (log-odds) from the true motif, scans each mutated sequence, and evaluates detection using a threshold based on the true motif’s self-score:
- Computes a per-sequence maximum PWM scan score.
- Flags `motif_detected` if `pwm_score >= 0.8 * true_score`.

Outputs:
- A motif detection rate vs divergence plot (separately for subs-only and subs+indels).

### 7) Combined figure + written interpretation
- Produces a combined 3-panel figure summarizing Tasks A/B/C.
- Contains markdown cells interpreting the plots and discussing approximate reliability thresholds.

## How to run

1. Open `code.ipynb` in VS Code or Jupyter.
2. Run cells from top to bottom.

### Requirements
- Python 3.x
- `numpy`, `pandas`, `matplotlib`

If packages are missing, install (example):

```bash
pip install numpy pandas matplotlib
```

## Notes
- Results are stochastic but should be reproducible due to fixed random seeds.
- The notebook already includes cell-level explanations and interpretations; this README only summarizes the structure and outputs.
