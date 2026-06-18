# Electrostatic Capacitor Simulation — COMSOL Multiphysics

2D electrostatic analysis of a parallel-plate capacitor with dielectric media, completed for the Computational Photonics II Laboratory at the University of Kassel.

## Overview
This project simulates the electric potential distribution and space charge effects in a capacitor structure using COMSOL Multiphysics, based on the Finite Element Method (FEM).

## Governing Equation
Gauss's Law:

-∇·(ε∇V) = ρ

E = -∇V

Where V is electric potential, ε is material permittivity, and ρ is space charge density.

## Cases studied
- **Case I — Ideal Capacitor:** No space charge, dielectric (Glass/PVC) between metal plates
- **Case II — Uniformly Charged Dielectric:** Constant space charge density ρ = 0.3 C/m³
- **Case III — Non-Uniform Charge Distribution:** Linearly varying charge density using an analytic function

## Materials
- Air: εr = 1
- Glass: εr = 4.2
- PVC: εr = 3
- Electrodes: Aluminum

## Tools
- COMSOL Multiphysics 6.3 (AC/DC Module — Electrostatics)
- Finite Element Method (FEM)

## Files
- `electrostatic-2D.mph` — COMSOL model file
- Report/observations — written analysis and results

## Author
**Sanjana Banala** — M.Sc. Electrical Communication Engineering, University of Kassel
