## Version 0.2: High-Density Simulation & Stability
*(File: in.settle_v0.2.lj)*

This version increases the simulation's complexity and robustness, moving from a sparse system to a dense one.

### Key Changes from v0.1
* **Particle Count:** Increased from 200 to **2200 particles** to simulate a dense granular bed.
* **Particle Creation:** Particles are now created in three stacked regions to provide a more stable and dense initial configuration.
* **Timestep Ramping:** A multi-stage run is implemented. The simulation starts with a very small timestep (0.5 microseconds) to safely resolve initial particle overlaps and "ramps up" to the final 10-microsecond timestep. This is critical for preventing simulation crashes.
* **Total Time:** The total simulation time is extended from 0.5s to **2.275s** to allow the dense system to fully settle.
* **Friction:** The particle and wall friction coefficient (`xmu`) has been reduced from 0.5 to **0.3**, making the grains "slipperier."
* **Stability:**
    * `velocity all set 0 0 0`: Ensures all particles start from rest.
    * `thermo_modify lost warn`: Adds a check to warn if any particles are lost during the run.
* **Code Fix:** The `set ... diameter` command was corrected to use an explicit `diameter` variable for better code clarity. The particle size remains 5mm.