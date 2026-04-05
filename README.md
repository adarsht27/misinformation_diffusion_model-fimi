# sbfc-fimi-abm

> An agent-based model of Foreign Information Manipulation and Interference (FIMI) diffusion in social networks, built in NetLogo. Extends the SBFC framework (Sulis & Tambuscio, 2020) with asymmetric spreading and forgetting dynamics to simulate contrasting EU information ecosystems.

---

## Table of Contents

- [Background](#background)
- [What This Project Adds](#what-this-project-adds)
- [Model Overview](#model-overview)
- [Parameters](#parameters)
- [Scenarios](#scenarios)
- [Policy Implications](#policy-implications)
- [Requirements](#requirements)
- [How to Run](#how-to-run)
- [Repository Structure](#repository-structure)
- [Limitations](#limitations)
- [Citation](#citation)
- [License](#license)

---

## Background

The European Union faces systematic Foreign Information Manipulation and Interference (FIMI) campaigns from state and non-state actors. Russia, China, and others exploit declining institutional trust, echo chambers, and weak fact-checking infrastructure to amplify polarisation and undermine democratic processes.

Existing counter-FIMI tools focus on detecting individual false claims or monitoring specific campaigns. What is harder to answer is a structural question: **which interventions would be most cost-effective at building societal resilience at scale?** Should EEAS prioritise fact-checking infrastructure, reducing information segregation, or rebuilding institutional trust?

This project uses agent-based modelling to explore that question — simulating how misinformation and corrective information compete across different information environments.

---

## What This Project Adds

The original SBFC model by Sulis & Tambuscio (2020) is a solid foundation, but its symmetric design produces an unrealistic result: fact-checkers always dominate, regardless of how hostile the information environment is. Through close reading of the code, three structural reasons were identified:

| Issue | Original Model | This Extension |
|---|---|---|
| Spreading rate | Misinformation and fact-checking share one `beta-spreadingRate` | Separate `beta-fc-spread` for fact-checkers |
| Forgetting rate | All agents forget at the same rate (`pForget`) | Separate `pForget-FC` for fact-checkers |
| State transitions | One-directional: B → FC only | Asymmetric decay allows FC → S, representing erosion of corrective behaviour |

These two new parameters — `beta-fc-spread` and `pForget-FC` — allow the model to produce qualitatively distinct diffusion regimes for different socio-political contexts, which the original could not.

---

## Model Overview

The SBFC framework models information diffusion as a competition between a hoax and its debunking across a social network. Each agent occupies one of three states at any time step:

- **S — Susceptible**: has not yet encountered the hoax or its correction
- **B — Believer**: accepts the hoax and may spread it to neighbours
- **FC — Fact-Checker**: has verified or rejected the hoax and spreads corrections

State transitions are governed by local peer interactions and four (now six) parameters. The network is generated using the **Barabási–Albert preferential attachment algorithm**, producing a scale-free topology that reflects real online ecosystems — a small number of highly connected hubs play a disproportionate role in diffusion.

Agents are coloured in the NetLogo interface:
- 🔵 Blue = Believer
- 🔴 Red = Fact-Checker
- ⚫ Grey = Susceptible

---

## Parameters

### Original parameters (Sulis & Tambuscio, 2020)

| Parameter | Variable name | Description |
|---|---|---|
| Spreading rate | `beta-spreadingRate` | Probability a Believer spreads the hoax to a neighbour |
| Hoax credibility | `alpha-hoaxCredibility` | Persuasive advantage of the hoax over corrections |
| Verification probability | `pVerify` | Probability a Believer converts to Fact-Checker |
| Forgetting rate | `pForget` | Probability a Believer returns to Susceptible |

### New parameters (this extension)

| Parameter | Variable name | Description |
|---|---|---|
| Fact-checker spreading rate | `beta-fc-spread` | Independent spreading rate for corrective information |
| Fact-checker forgetting rate | `pForget-FC` | Probability a Fact-Checker returns to Susceptible |

**Why these matter:** Empirical research shows that corrections circulate more slowly than misinformation (lower emotional salience, reduced engagement) and decay faster (cognitive anchoring keeps false beliefs salient). These two parameters capture both effects.

---

## Scenarios

Two contrasting scenarios are included, calibrated to represent different EU information environments:

### Scenario 1 — Vulnerable Information Ecosystem

Represents low-trust, high-polarisation environments with weak fact-checking capacity and strong exposure to FIMI (e.g. fragmented online communities, poor media literacy).

| Parameter | Value | Rationale |
|---|---|---|
| `beta-spreadingRate` | 0.55 | High virality on unmoderated platforms |
| `beta-fc-spread` | 0.10 | Corrections spread slowly in low-trust contexts |
| `alpha-hoaxCredibility` | 0.85 | Highly persuasive FIMI narratives |
| `pVerify` | 0.05 | Very limited willingness to verify |
| `pForget` | 0.03 | False beliefs decay slowly |
| `pForget-FC` | 0.30 | Corrective behaviour decays rapidly |

**Outcome:** Believers stabilise at ~70–80% of the population. Fact-checkers remain a small minority. Misinformation persists.

### Scenario 2 — Resilient Information Ecosystem

Represents high-trust environments with strong institutional response capacity, widespread media literacy, and active platform cooperation.

| Parameter | Value | Rationale |
|---|---|---|
| `beta-spreadingRate` | 0.45 | Moderated virality through platform interventions |
| `beta-fc-spread` | 0.55 | Strong capacity for rapid correction |
| `alpha-hoaxCredibility` | 0.25 | Low persuasive power in high-trust contexts |
| `pVerify` | 0.40 | High engagement with verification |
| `pForget` | 0.20 | Misinformation decays quickly |
| `pForget-FC` | 0.05 | Corrective behaviour persists |

**Outcome:** Believers disappear by ~50 ticks. Fact-checkers stabilise at ~90% of the population.

These two outcomes were **indistinguishable** under the original model. The extended model differentiates them clearly.

---

## Policy Implications

The model is framed around a policy question for EEAS STRAT.1 and STRAT.4: which counter-FIMI investments yield the highest return? Key findings:

1. **Reactive fact-checking alone is insufficient** in low-trust environments — structural resilience building (media literacy, public service media, trust in institutions) is needed upstream.
2. **`pVerify` is the highest-leverage parameter** — small increases in verification behaviour produce disproportionate system-wide improvements.
3. **Corrective information must persist** — platform-level ranking and visibility of fact-checks directly reduces `pForget-FC` and improves resilience.
4. **Pre-bunking reduces `alpha`** — early warnings and inoculation narratives diminish hoax credibility before it takes hold.
5. **High-degree network nodes matter** — scale-free topology means a small number of accounts disproportionately drive diffusion. Monitoring hubs disrupts cascades.

---

## Requirements

- [NetLogo 7.0.2](https://ccl.northwestern.edu/netlogo/) or later
- The `nw` (Networks) extension — bundled with NetLogo, no separate installation needed

---

## How to Run

1. Clone or download this repository
2. Open NetLogo 7.0.2
3. Open `models/Updated_SBFC_FIMI.nlogo` via **File → Open**
4. Click **Setup** to initialise the network and agent states
5. Click **Go** to run the simulation (stops automatically at 300 ticks)
6. Adjust sliders to explore different parameter configurations
7. Use the built-in BehaviorSpace tool (**Tools → BehaviorSpace**) for parameter sweeps

To run the original baseline model for comparison, open `models/Original_Sulis2020.nlogo`.

---

## Repository Structure

```
sbfc-fimi-abm/
├── README.md
├── LICENSE
├── models/
│   ├── Original_Sulis2020.nlogo          # Baseline model by Sulis & Tambuscio (2020)
│   └── Updated_SBFC_FIMI.nlogo           # Extended model (this project)
├── docs/
│   ├── report.docx                       # Full project report
│   └── presentation.pptx                 # Project presentation
```

---

## Limitations

- The Barabási–Albert network does not reproduce echo chambers or linguistic/ideological clustering that affect real-world diffusion
- No re-conversion path from Fact-Checker to Believer via active misinformation targeting
- Behavioural assumptions are simplified — real users do not adopt or forget based solely on neighbour states
- Parameter values are illustrative, not calibrated to empirical country-level data

Despite these limitations, the extended model represents a meaningful improvement in behavioural realism and is suitable for structured exploration of counter-FIMI policy options.

---

## Citation

If you use or build on this work, please cite the original model:

```
Sulis, E., & Tambuscio, M. (2020). Simulation of misinformation spreading
processes in social networks: An application with NetLogo.
2019 IEEE International Conference on Data Science and Advanced Analytics (DSAA).
DOI: 10.1109/DSAA49011.2020.00086
```

Original model available at: http://modelingcommons.org/browse/one_model/6403

The theoretical SBFC framework is based on:

```
Tambuscio, M., Ruffo, G., Flammini, A., & Menczer, F. (2015).
Fact-checking effect on viral hoaxes: A model of misinformation spread in social networks.
Proceedings of the 24th International Conference on World Wide Web (WWW '15).
```

---

## License

The extensions and modifications in this repository are released under the [MIT License](LICENSE).

The base SBFC model is derived from Sulis & Tambuscio (2020), shared openly on the NetLogo Modeling Commons. All credit for the original framework belongs to the original authors.
