# HACHQ — Two-Threshold Actor–Critic Routing for Heterogeneous Queues with Switching Costs

Reference implementation for M. Ali and D. Efrosinin, *"Two-Threshold Actor-Critic
Routing for Heterogeneous Queues with Switching Costs"*, IEEE, 2026. [DOI pending]
Extended treatment: [MSc thesis, JKU Linz](https://epub.jku.at/obvulihs/download/pdf/14558869)

HACHQ extends ACHQ (Jali et al., AISTATS 2024) with (i) a switching-cost-aware
objective and (ii) a two-threshold hysteresis policy - activation `q1`,
deactivation `q0` - learned per controlled server, without model knowledge.

## Install
```bash
pip install -r requirements.txt
```

## Reproducing the paper


| Paper result | Notebook |
|---|---|
| Fig. 1 - Two-server cost sweep | [`02_two_server.ipynb`](notebooks/02_two_server.ipynb) |
| Fig. 2 + Table I - Training dynamics (5 seeds) | [`03_training_dynamics.ipynb`](notebooks/03_training_dynamics.ipynb) |
| Figs. 3–4 - RVI optimum and suboptimality gap | [`04_rvi_validation.ipynb`](notebooks/04_rvi_validation.ipynb) |
| Fig. 5 - Multi-server scaling | [`05_multiserver.ipynb`](notebooks/05_multiserver.ipynb) |
| Simulator validation | [`01_simulator_tests.ipynb`](notebooks/01_simulator_tests.ipynb) |

Cached outputs are available in `results/`, so figures can be rebuilt without retraining.

## Setup (Section V-A)

- **Learning rates:** \(\alpha_w = \alpha_\theta = 10^{-3}\), scaled as \(1/\sqrt{k}\) for \(k > 2\).
- **Other parameters:** \(\kappa = 1.0\), \(\beta = 10^{-2}\), gradient clipping at \(\pm 0.5\).
- **Training:** \(6 \times 10^5\) steps, buffer size \(l_M = 100\).
- **Random seeds:** Training seeds 0–4; evaluation seeds 1000–1004.
- **Two-server setup:** \(\mu_1 = 10,\ \mu_2 = 1\).
- **Multi-server setup:** \(\mu = \operatorname{linspace}(10,1,k)\).
- **Initial thresholds:** ACHQ: \(q_1 = 5\); HACHQ: \((q_0,q_1) = (2,6)\).

## Project structure

| File | Description |
|---|---|
| `simulator.py` | Uniformised continuous-time Markov chain (CTMC) simulator |
| `achq.py` | ACHQ baseline implementation |
| `hachq.py` | HACHQ implementation proposed in this paper |
| `multiserver.py` | Multi-server experiments |
| `rvi.py` | Relative value iteration (RVI) and exact optimum |
| `metrics.py` | Logging and paired-seed statistics |
| `figures.py` | Publication-quality figures |

## Notes
- Averages are over uniformised time steps (R = λ + Σμᵢ), matching the RVI yardstick.
- A server that goes idle because the queue emptied is not a decision, so no C₀ is
  charged and it is not counted in the switching rate.
- ACHQ learns a negative threshold at some configurations; the base objective does
  not penalise over-activation, so "always activate" is a stationary point. 
- `evaluate_rsrt` is an extra baseline from Jali et al., not reported in the paper.

## Citation
```bibtex
@inproceedings{ali2026hachq, ... }
```
MIT licensed.
