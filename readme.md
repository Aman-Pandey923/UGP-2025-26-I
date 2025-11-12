## Version 0.3: Impulse & Mixing Simulation
*(File: in.settle_v0.3.lj)*

This version extends the simulation to study the system's response to external forces, specifically modeling agitation, shaking, or mixing. The simulation is now a multi-stage process.

### Key Changes from v0.2
* **Multi-Stage Simulation:** The script now executes in three phases:
    1.  **Settling Phase:** (Identical to v0.2) Creates a stable, settled bed of 2200 particles.
    2.  **Agitation Phase:** Applies two sequential, high-velocity "jolts" to the entire system.
    3.  **Re-settling Phase:** Allows the agitated particles to settle again under gravity.
* **Data Snapshots:**
    * `settled_before_impulse.data`: A new file is written to save the state of the stable bed *before* any impulses are applied. This serves as the "before" state.
    * `final_after_impulse.data`: A final data file is written to save the "after" state, capturing the particle configuration after all mixing and re-settling is complete.
* **Physics (Impulses):**
    * Two `velocity all set ...` commands are used to apply sudden velocity changes to all particles, simulating a shake or jolt.
* **Additional Runtime:** 3.0 seconds of simulation time (300,000 steps) are added to model the system's response to the impulses and its subsequent re-settling.