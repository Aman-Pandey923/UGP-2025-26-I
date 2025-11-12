## Version 0.4: Refined Single-Impulse Study
*(File: in.settle_v0.4.lj)*

This version refactors the agitation phase (v0.3) to create a more controlled and analyzable experiment. The goal is to observe the bed's response to a single, clean jolt rather than a complex shake.

### Key Changes from v0.3
* **Simplified Agitation:** The two sequential, multi-directional impulses have been replaced by a **single, uniform, upward impulse** (`velocity all set 0.0 0.0 1.5`).
* **Experiment Type:** This changes the simulation from a chaotic "shaking" model to a more controlled "upward tap" model. This allows for a clearer analysis of the bed's expansion and re-settling dynamics.
* **Consolidated Runtime:** The post-impulse simulation is now a single `run` command of 150,000 steps (1.5 seconds), which captures both the immediate response to the jolt and the subsequent re-settling process.