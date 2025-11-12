# Project: Granular Settling Simulation (v0.1)

**Purpose:** This LAMMPS script simulates the "free fall" and subsequent settling of spherical granular particles under gravity. The particles are created in the upper half of a box and allowed to fall and form a settled bed at the bottom.

---

## Script Breakdown

### 1. Initialization & Setup
* `units si`: We are using standard SI units (meters, kilograms, seconds, etc.).
* `atom_style sphere`: Defines our particles as spheres, which means LAMMPS will track their radius, density, and rotational motion.
* `boundary f f f`: Sets all boundaries (x, y, z) to be **fixed** (non-periodic). This models a closed box.
* `newton off`: This is standard for granular pair styles. It tells LAMMPS not to compute Newton's third law internally, as the granular contact models handle pairwise forces explicitly.

### 2. Simulation Domain & Particle Creation
* `region box...`: This defines the total simulation box, which is 0.1m x 0.1m x 0.2m (10cm x 10cm x 20cm).
* `create_box 1 box`: This "creates" the empty simulation box, allocating memory for one type of particle.
* `region creation...`: This defines a smaller region *within* the main box (from z=0.12m to z=0.19m), which is in the upper half.
* `create_atoms 1 random 200...`: This is where the particles are born. It creates **200** particles of type 1, placing them randomly *only* inside the `creation` region.

### 3. Physical Properties
**Particles:**
* `variable density equal 2500.0`: Sets particle density to 2500 kg/m³, similar to sand or glass.
* `variable radius equal 0.005`: Sets particle radius to 5mm (1cm diameter).
* `set group all diameter/density...`: These commands apply the density and diameter (using the radius variable) to all 200 created particles.

**Interactions (The Physics):**
* `pair_style gran/hertz/history...`: This is the core of the simulation. It selects the **Hertzian contact model** for granular particles. This model calculates the normal and tangential forces (friction) during a collision based on material properties.
* `pair_coeff * *`: This applies the Hertzian model to all possible interactions (in this case, only type 1 with type 1).

### 4. Simulation Environment & Dynamics
* `gravity 9.81 vector 0 0 -1`: This applies a constant downward gravitational force (9.81 m/s²) to all particles.
* `fix ... wall/gran ...`: These three `fix` commands create the **six containing walls** (bottom/top z, front/back y, left/right x). They use the *same* Hertzian contact model as the particles, so particle-wall collisions are handled physically.
* `fix integrate all nve/sphere`: This is the time integrator. It tells LAMMPS to update the position and velocity of all particles at each step. `nve/sphere` is specifically for spherical particles and correctly updates both their translational and rotational motion.

### 5. Execution & Output
* `timestep 0.00001`: Sets the simulation timestep to 10 microseconds (1e-5 s). This must be very small to accurately resolve the brief moment of a particle collision.
* `thermo 1000`: Prints thermodynamic data (like kinetic energy, potential energy) to the screen and log file every 1000 steps.
* `dump viz all custom 500...`: This is the main output for visualization. It saves a "snapshot" of the simulation to the file `dump.grain` every 500 steps. It stores each particle's ID, type, position (x,y,z), radius, and velocity (vx,vy,vz). This file can be read by visualization tools like **OVITO**.
* `run 50000`: This runs the simulation for 50,000 steps.
    * (Total simulation time: 50,000 steps * 1e-5 s/step = **0.5 seconds**)
* `write_data final_settled.data`: After the run finishes, this command saves the final state of all particles into a single data file.