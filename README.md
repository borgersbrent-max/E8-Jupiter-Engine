# E8-Jupiter-Engine (archived)

**This repository is archived.** For the controlled tests of E8 structure, see the negative-results note in time-matter-engine:
**[NEGATIVE-RESULTS.md](https://github.com/borgersbrent-max/time-matter-engine/blob/main/NEGATIVE-RESULTS.md)**

That note does not cover this repository. The audit below is the record for it.

## What the code computes

`jupiter_engine_multiseed.py` draws a 100 × 240 matrix of random normal numbers and averages its rows into a 240-vector. It then applies 89 steps of

    state = state + (1/137.035) * sin(step / 17) * tanh(state)

and reports the length of the result, over 12,000 seeds.

An audit on 16 November 2026 (2,000 seeds) found:

- **The 240 values are random numbers, not E8 roots.** No lattice or root vector appears in the code.
- **The update acts on each value separately.** The 240 values never interact.
- **The output is a fixed multiple of plain random sampling.** For every seed, the engine's output equals the length of the averaged random vector (about √(240/100) ≈ 1.55) times a near-constant factor of 1.0585–1.0589. The constants 17, 89 and 1/137 change only that factor.
- **`monster_multi_seed_control.py`** adds a gate that rescales the vector by a function of its own length. That changes the scale factor, not the picture.

So the 12,000-seed statistics measure random sampling, not E8 or 5+1 structure. Any 5+1 framework would need to be written down as explicit equations and tested against matched controls, as in time-matter-engine.

## Note

The GitHub Actions workflow here runs `pytest`, but the repository has no tests, so it fails on every run.
