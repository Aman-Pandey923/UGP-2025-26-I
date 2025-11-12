## Version 0.5: Stratified 3-Grain Mixing Simulation
*(File: in.settle_v0.5.lj)*

This is a major feature update, converting the simulation into a multi-phase experiment to study the mixing of a pre-stratified granular bed.

### Key Changes from v0.4
* **4-Phase Simulation:** The script now runs in four distinct phases:
    1.  **Initial Settle:** A uniform bed of 2200 particles (all Type 1) is settled.
    2.  **Type Assignment:** Physics is paused, and particles are assigned new types (1, 2, or 3) and densities (2500, 2000, 1500 kg/m³) based on their Z-position.
    3.  **Re-Settle:** A brief run allows the new, density-stratified system to stabilize.
    4.  **Impulse & Mix:** A localized impulse is applied to study mixing.

* **Stratified Bed:** The simulation now creates a bed with three layers of different densities (heavy, medium, light) to model gravitational segregation.

* **Sinusoidal Boundaries:** The layers are not flat. Type assignment is based on `atom-style` variables that define wavy, sinusoidal interfaces between the layers.

* **Localized Impulse:** The previous uniform, box-wide impulse has been replaced by a **localized, downward impact** in a central cylindrical region, simulating an object being dropped into the grains.

* **Mixing Analysis:** The simulation now quantifies mixing by:
    * Tracking the average Z-position of each grain type (`compute z1`, `c_z2`, `c_z3`).
    * Writing this data over time to a new file, `mixing_evolution.txt`, for easy plotting and analysis.

* **New Data Outputs:** The script saves the state at the end of each key phase:
    * `settled_single_type.data` (After Phase 1)
    * `settled_three_types.data` (After Phase 3, the "before" mixing state)
    * `final_mixed_state.data` (After Phase 4, the "after" mixing state)


## Version 0.6: High-Density, High-Impact Simulation
*(File: in.settle_v0.6.lj)*

This version scales the simulation (v0.5) to a much larger, denser system and models a high-energy impact event.

### Key Changes from v0.5
* **Particle Count:** The total number of particles has been **tripled from 2200 to 6600**. This creates a denser, higher settled bed.
* **Impulse Strength:** The localized impulse is now **10.5 times stronger**, with the impact velocity increased from -2.0 m/s to **-21.0 m/s**. This models a high-velocity, violent impact.
* **Post-Impulse Runtime:** The simulation time after the impact has been **halved from 2.0s to 1.0s** to focus on the immediate mixing dynamics.
* **Layer Boundary Discrepancy (NOTE):** The *comments* for the sinusoidal layer boundaries (`h1`, `h2`) were updated (to 4cm/8cm), but the *code values* remain at `0.03` and `0.06`. Since the 6600-particle bed will settle much higher, these fixed boundaries will now be much lower relative to the bed, leading to a different initial particle-type distribution (a much larger bottom layer).