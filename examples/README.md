# ACP Simulation Examples

This directory contains example files and complete workflows for setting up molecular dynamics simulations of Anion Conducting Polyelectrolytes (ACP) using GROMACS.

These examples support the research presented in **"Origins of Enhanced Ion Transport in Nanostructured Anion Conducting Polyelectrolytes"**.

## Installation & Setup

Set up force field environment:
```bash
export GMXLIB=$PWD/../force_field:$GMXLIB
```

## Contents

### Example Input Files
- `HROMP-single.pdb` - Single ACP polymer chain structure
- `Br.gro` - Bromide ion coordinate file
- `water.gro` - Water molecule coordinate file

### Example Topology Files
- `topol_example.top` - Example topology file with proper force field paths


## Complete Simulation Workflow

### Step 1: System Preparation

```bash
# Load environment
export GMXLIB=$PWD/../force_field:$GMXLIB

# Generate topology from PDB structure
# Select appropriate force field when prompted (typically OPLS-AA)
gmx pdb2gmx -f HROMP-single.pdb -o polymer.gro -p topol.top
```

**Important:** Update the force field paths and parameters in the generated topology file (`topol.top`).

### Step 2: Build Simulation Box

```bash
# Create simulation box (example: 20x20x20 nm)
gmx editconf -f polymer.gro -o box.gro -box 20 20 20 -c

# Add multiple polymer chains (example: 24 additional chains for 25 total)
gmx insert-molecules -f box.gro -ci polymer.gro -nmol 24 -o polymers_chain25.gro -try 1000
```

### Step 3: Add Ions and Solvent

```bash
# Add bromide ions (ensure system neutrality)
gmx insert-molecules -f polymers_chain25.gro -ci Br.gro -nmol 1000 -o polymer_chain25_ions.gro -try 500

# Add water molecules (optional, for hydrated systems)
gmx insert-molecules -f polymer_chain25_ions.gro -ci water.gro -nmol 2000 -o polymer_chain25_ions_water.gro -try 500
```

**Important:** Update the `[molecules]` section in `topol.top` to reflect the actual number of each component in the system.

### Step 4: Energy Minimization

```bash
# Prepare energy minimization
gmx grompp -f ../simulation_params/ENERGY_MIN.mdp -c polymer_chain25_ions_water.gro -p topol.top -o em.tpr -maxwarn 1

# Run energy minimization
gmx mdrun -deffnm em -v
```

**Expected output:** `em.gro`, `em.edr`, `em.log`

### Step 5: Equilibration

```bash
# NPT equilibration phase 1
gmx grompp -f ../simulation_params/EQUILIBRATION-1.mdp -c em.gro -p topol.top -o eq1.tpr -maxwarn 1
gmx mdrun -deffnm eq1 -v

# NPT equilibration phase 2
gmx grompp -f ../simulation_params/EQUILIBRATION-2.mdp -c eq1.gro -p topol.top -o eq2.tpr -maxwarn 1
gmx mdrun -deffnm eq2 -v
```

**Expected outputs:** 
- Phase 1: `eq1.gro`, `eq1.edr`, `eq1.log`, `eq1.trr`
- Phase 2: `eq2.gro`, `eq2.edr`, `eq2.log`, `eq2.trr`

### Step 6: Production Run

```bash
# Prepare production simulation
gmx grompp -f ../simulation_params/PRODUCTION.mdp -c eq2.gro -p topol.top -o md.tpr -maxwarn 1

# Run production simulation
gmx mdrun -deffnm production -v
```

**Expected outputs:** `production.gro`, `production.edr`, `production.log`, `production.trr`

## Expected Outputs and Analysis

### Key Output Files

| File Type | Description | Usage |
|-----------|-------------|--------|
| `.gro` | Final coordinates | Structure visualization, analysis starting points |
| `.trr` | Compressed trajectory | Main trajectory for analysis |
| `.edr` | Energy data | Energy analysis, system stability monitoring |
| `.log` | Simulation log | Performance metrics, error checking |


### Typical Runtimes
Computational requirements vary based on system size and available hardware resources:
- **Energy Minimization:** 5-30 minutes
- **Equilibration:** 24-72 hours per phase
- **Production (100 ns):** 48-96 hours

