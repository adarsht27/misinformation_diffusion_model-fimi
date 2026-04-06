# Models

This folder contains the NetLogo model files for this project.

## Files

### `original_Sulis2020.nlogo`
The baseline SBFC model by Sulis & Tambuscio (2020), included here unchanged for reference and comparison. All credit belongs to the original authors.

- Source: [NetLogo Modeling Commons](http://modelingcommons.org/browse/one_model/6403)
- Parameters: `beta-spreadingRate`, `alpha-hoaxCredibility`, `pVerify`, `pForget`
- Networks supported: Barabási–Albert, Erdős–Rényi

### `updated_SBFC_FIMI.nlogo`
The extended model developed in this project. Adds two new parameters to introduce asymmetric dynamics between misinformation and fact-checking diffusion.

**New parameters:**
- `beta-fc-spread` — independent spreading rate for corrective information
- `pForget-FC` — independent forgetting rate for Fact-Checkers

Built and tested with **NetLogo 7.0.2**. The `nw` (Networks) extension is required — it is bundled with NetLogo and needs no separate installation.

## How to Run

1. Open NetLogo 7.0.2
2. Open the model file via **File → Open**
3. Click **Setup** to initialise agents and network
4. Click **Go** to run (auto-stops at 300 ticks)
5. Adjust sliders to explore parameter space
6. Use **Tools → BehaviorSpace** for automated parameter sweeps