# BoundaryDefense

Acoustic swarm simulation and empirical pipeline for:

**The Intruder's Dilemma: Probabilistic 3D Boundary Defense using Brownian Acoustic Drone Swarms**

This repository combines real drone telemetry with acoustic modeling and Monte Carlo simulation to study how a defender swarm detects an intruder in 3D airspace.

## Project Overview

The codebase implements an end-to-end workflow:

1. Parse and clean real flight telemetry (AirData CSV exports).
2. Align telemetry with onboard acoustic signals (or generate synthetic audio fallback).
3. Calibrate intruder source-level and defender noise-floor models.
4. Run vectorized Monte Carlo experiments for probabilistic detection.
5. Produce publication-ready figures for attacker-dilemma, scalability, and altitude-speed sensitivity.

Two simulation formulations are provided:

- `Base.ipynb`: Brownian defender swarm in a defended 3D volume.
- `Wall.ipynb`: Structured "acoustic wall" formation (parallelepiped layout with drift).

## Repository Structure

```text
BoundaryDefense/
├─ Base.ipynb
├─ Wall.ipynb
├─ README.md
└─ data/
   ├─ mavic_air2_flight.csv
   ├─ mini3pro_01_flight.csv
   └─ mini3pro_02_flight.csv
```

## Data Inputs

### Required telemetry (included)

- `data/mavic_air2_flight.csv` (intruder telemetry)
- `data/mini3pro_01_flight.csv` (defender telemetry)
- `data/mini3pro_02_flight.csv` (defender telemetry)

### Optional audio (not required to run)

The notebooks are configured for defender WAV files:

- `data/mini3pro_01_audio.wav`
- `data/mini3pro_02_audio.wav`

If these files are missing, the pipeline falls back to synthetic audio generation so experiments still run end-to-end.

## Environment Setup

Recommended: Python 3.10+ in a fresh virtual environment.

```bash
python -m venv .venv
# Windows PowerShell
.\.venv\Scripts\Activate.ps1

pip install --upgrade pip
pip install numpy pandas scipy matplotlib seaborn scikit-learn tqdm jupyter
pip install librosa pyproj
```

Notes:

- `librosa` and `pyproj` are optional in code (graceful fallback is implemented).
- Installing both is recommended for full fidelity and geospatial accuracy.

## Running the Notebooks

1. Open either notebook in VS Code or Jupyter:
   - `Base.ipynb`
   - `Wall.ipynb`
2. Review and update `DATA_CONFIG` paths if your local dataset differs.
3. Run cells sequentially from top to bottom.

Both notebooks are organized in three phases:

- **Phase 1: Empirical Grounding**
  CSV parsing, coordinate conversion, waveform handling, synchronization, and spectral features.
- **Phase 2: Simulation Engine**
  OOP model for environment, defenders, intruder, acoustic physics, and vectorized Monte Carlo core.
- **Phase 3: Experiments**
  Main paper plots and derived metrics.

## Main Experiments

The notebooks include three primary experiments:

1. **Attacker's Dilemma (U-curve):** detection probability versus intruder speed.
2. **Scalability:** defender count sweep to estimate required swarm size for target detection reliability.
3. **Altitude-Speed Heatmap:** detection landscape over intruder altitude and speed.

Typical outputs are saved under `figures/` and include:

- U-curve and smoothed trend plots.
- Detection-vs-defender-count plots (linear and log-scale variants).
- 2D heatmap and 3D surface views for altitude-speed sensitivity.
- CSV exports for experiment summaries.

## Configuration Highlights

Core controls are defined directly in notebook cells:

- `DATA_CONFIG` for telemetry/audio paths and figure output folder.
- `SIM_CONFIG` for environment dimensions, defender counts/sweeps, intruder speed sweeps, SNR threshold, and Monte Carlo budget.

For reproducibility:

- Keep random seeds fixed (already set in notebook setup).
- Re-run full notebooks after changing `SIM_CONFIG` parameters.
- Prefer consistent Python package versions across machines.

## Reproducibility Checklist

- Use the provided telemetry CSV files.
- Ensure all setup/import cells run without errors.
- Run each notebook top-to-bottom in a clean kernel.
- Verify figures are generated in `figures/`.
- Archive exported CSV/figures for paper appendix and review response.

## Citation

If you use this repository in academic work, cite the associated paper/manuscript and reference this repository.

```bibtex
@inproceedings{boundarydefense,
  title        = {The Intruder's Dilemma: Probabilistic 3D Boundary Defense using Brownian Acoustic Drone Swarms},
  author       = {Vincenzo Sammartino, Nathanael Denis, Omar Ibrahim, Roberto Di Pietro},
  booktitle    = {Proceedings of the 31st European Symposium on Research in Computer Security (ESORICS) 2026},
  year         = {2026},
  note         = {Code repository}
}
```
