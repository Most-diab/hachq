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

| Paper | Notebook 
|---|---|---|
| Fig. 1 — two-server cost sweep | `notebooks/02_two_server.ipynb` 
| Fig. 2 + Table I — training dynamics, 5 seeds | `notebooks/03_training_dynamics.ipynb` 
| Figs. 3–4 — RVI optimum and suboptimality gap | `notebooks/04_rvi_validation.ipynb` 
| Fig. 5 — multi-server scaling | `notebooks/05_multiserver.ipynb`
| Simulator validation | `notebooks/01_simulator_tests.ipynb` 

Cached outputs are in `results/`, so figures can be rebuilt without re-training.

## Setup (Section V-A)
κ = 1.0 · α_w = α_θ = 10⁻³ (∝ 1/√k for k > 2) · β = 10⁻² · gradient clip ±0.5
· 6×10⁵ training steps · buffer l_M = 100 · seeds 0–4 (eval seeds 1000–1004)
· two-server μ₁ = 10, μ₂ = 1 · multi-server μ = linspace(10, 1, k)
· initial thresholds: ACHQ q₁ = 5; HACHQ (q₀, q₁) = (2, 6)

## Layout
`simulator.py` uniformised CTMC · `achq.py` baseline · `hachq.py` this paper ·
`multiserver.py` k servers · `rvi.py` exact optimum · `metrics.py` logging and
paired-seed statistics · `figures.py` publication figures

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
