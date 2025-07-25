# Origins of Enhanced Ion Transport in Nanostructured Anion Conducting Polyelectrolytes

This code supports the research presented in "Origins of Enhanced Ion Transport in Nanostructured Anion Conducting Polyelectrolytes".

This repository contains the complete simulation setup, force field parameters, and configuration files for molecular dynamics simulations of nanostructured anion conducting polyelectrolytes (ACPs) using GROMACS with a modified OPLS-AA force field. 

## Table of Contents
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Usage Instructions](#usage-instructions)
- [Repository Structure](#repository-structure)

## System Requirements

### Software Dependencies
- **GROMACS** >= 2022.4 (see [GROMACS official website](https://www.gromacs.org/) for installation)
- **Python** >= 3.8 (optional, for analysis)

### Operating Systems
This software has been tested on:
- Linux 4.18.0-305.57.1.el8_4.x86_64 (CentOS 8)
- Other Linux distributions should work with GROMACS support

## Installation Guide

### Repository Setup

1. **Clone or download this repository**:
   ```bash
   git clone https://github.com/gsun33/Nano-ACP.git
   cd Nano-ACP
   ```

2. **Install GROMACS**: Follow the [official GROMACS installation guide](https://manual.gromacs.org/current/install-guide/index.html)

3. **Set environment variables**:
   ```bash
   export GMXLIB=$PWD/force_field:$GMXLIB
   ```

### Installation Time
- Repository setup: < 1 minute
- GROMACS installation: varies by system (see official documentation)


## Usage Instructions

For complete simulation workflows, please refer to the detailed documentation in [`examples/README.md`](examples/README.md).

### Basic Workflow Overview
1. **System Preparation**: Use provided PDB files and setup scripts
2. **Topology Generation**: Apply modified OPLS-AA force field
3. **Simulation**: Follow standard GROMACS simulation protocol
4. **Analysis**: Use provided analysis tools or standard GROMACS utilities

### Force Field Details

This simulation uses a modified OPLS-AA force field specifically parameterized for anion conducting polyelectrolytes. The force field parameters are located in the `force_field/oplsaa.ff/` directory.

For detailed parameter definitions and validation, please refer to the research paper.


For GROMACS-specific commands and procedures, consult the [official GROMACS documentation](https://manual.gromacs.org/).

## Repository Structure

```
Nano-ACP/
├── README.md                   # This file
├── force_field/                # Modified OPLS-AA force field
│   └── oplsaa.ff/              # Force field parameters
├── simulation_params/          # GROMACS MDP files
│   ├── ENERGY_MIN.mdp          # Energy minimization
│   ├── EQUILIBRATION-1.mdp     # NPT/NVT equilibration 
│   ├── EQUILIBRATION-2.mdp     # NPT equilibration
│   └── PRODUCTION.mdp          # Production simulation
└── examples/                   # Example files and complete workflow
    ├── HROMP-single.pdb        # Single ACP polymer chain
    ├── Br.gro                  # Bromide ion coordinates
    ├── water.gro               # Water molecule coordinates
    ├── topol_example.top       # Example topology file
    └── README.md               # Detailed workflow instructions
```

