# Electrostatic Potential and Space Charge Effects in Dielectric Media
### Computational Photonics II — Lab Report

**Author:** Sanjana Banala
**Course:** Computational Photonics II Laboratory, Summer Semester 26
**Institution:** University of Kassel — FB 16 EECS / FB 10 Physics
**Instructors:** Prof. Dr. Jost Adam, Mohaddeseh Mehmandoust K.D.
**Tool used:** COMSOL Multiphysics 6.3 (Class Kit License)

---

## 1. Objective

The goal of this lab was to model and analyze the electric potential distribution and space charge effects inside a parallel-plate capacitor with a dielectric medium, using the Finite Element Method (FEM) in COMSOL Multiphysics. Three progressively complex cases were studied: an ideal capacitor with no space charge, a capacitor with a uniformly charged dielectric, and a capacitor with a non-uniform (linearly varying) charge distribution.

## 2. Governing Equations

The simulation is based on the 2D stationary electrostatic form of Gauss's Law:

```
-∇ · (ε∇V) = ρ
E = -∇V
```

Where:
- **V** — electric potential [V]
- **ε = ε₀εᵣ** — material permittivity
- **ρ** — space charge density [C/m³]
- **E** — electric field, derived from the gradient of potential

## 3. Geometry and Materials

The model geometry consisted of two horizontal metal plates with a dielectric layer between them, embedded in an air domain.

| Domain | Dimensions | Material | Relative Permittivity (εᵣ) |
|---|---|---|---|
| Air (outer domain) | 60 cm × 40 cm | Air | 1 |
| Upper electrode | 20 cm × 2 cm | Aluminum | — |
| Lower electrode | 20 cm × 2 cm | Aluminum | — |
| Dielectric core | 20 cm × 8 cm | Glass / PVC | 4.2 / 3 |

**Boundary conditions:**
- Lower plate: Ground, V = 0 V
- Upper plate: Terminal, V = 1 V

## 4. Methodology

The standard COMSOL modeling workflow was followed for each case:

1. **Geometry** — built using the Rectangle tool with Center-based positioning for the air domain, both electrodes, and the dielectric region
2. **Domain selections** — explicit selections were created for Air, Glass/PVC, and Metal domains to simplify material and physics assignment
3. **Material assignment** — Air, Glass, and Aluminum were assigned to their respective domain selections from the Built-In material library
4. **Physics setup** — the Electrostatics (es) interface was used under the AC/DC module, with Ground and Terminal boundary conditions applied to the lower and upper plates respectively
5. **Meshing** — a Free Triangular mesh was generated, with refinement tested at Fine, Extra Fine, and Extremely Fine resolutions to study the effect of mesh density on solution accuracy
6. **Study** — a Stationary study was computed for each case
7. **Postprocessing** — the default electric potential surface plot was extended with an Arrow Surface plot to visualize the electric field direction, and a 1D Line Graph was used to plot the potential profile along the center line of the capacitor

## 5. Case I — Ideal Capacitor

In this case, no space charge was present (ρ = 0), isolating the effect of the dielectric material alone on the potential distribution between the plates.

**Observations:**
- The electric potential transitioned smoothly and linearly from 0 V at the grounded lower plate to 1 V at the upper terminal plate, consistent with the expected behavior of an ideal parallel-plate capacitor with no free charge inside the dielectric.
- The electric field arrows (visualized via Arrow Surface) pointed uniformly from the high-potential plate to the ground plate, perpendicular to both electrode surfaces, confirming a near-uniform field distribution typical of parallel-plate geometries.
- The 1D line graph along the center line showed a clean linear potential gradient, matching the analytical solution for an ideal capacitor with a homogeneous dielectric and no internal charge.
- Switching the dielectric material between Glass (εᵣ = 4.2) and PVC (εᵣ = 3) did not change the shape of the potential profile, since with ρ = 0 the relative permittivity cancels out of the homogeneous Laplace equation along the 1D center line — only the field magnitude inside the dielectric would differ in a non-uniform geometry.

## 6. Case II — Uniformly Charged Dielectric

A constant space charge density of ρ = 0.3 C/m³ was introduced inside a smaller glass sub-domain (20 cm × 2 cm) at the center of the capacitor, using the Space Charge Density feature under the Electrostatics interface.

**Observations:**
- The presence of the space charge introduced a clear deviation from the linear potential profile seen in Case I.
- The 1D line graph showed a parabolic (curved) potential profile rather than a straight line, peaking near the center of the charged region — this matches the expected analytical behavior, since a constant ρ produces a quadratic term when integrating Poisson's equation twice.
- The electric field arrows showed a converging pattern around the charged dielectric region, with field lines bending toward the charge concentration rather than running uniformly between the plates as in Case I.
- Domain-based mesh refinement was applied, with a finer mesh size selected specifically within the charged sub-domain to better resolve the steeper potential gradient in that region, while the surrounding air domain retained a coarser mesh to reduce computational cost.

## 7. Case III — Non-Uniform Charge Distribution

A linearly varying space charge density was applied using an Analytic Function (`rho_fun`) defined as:

```
ρ(y) = -30·y  [µC/m⁴]
```

This function was entered under Definitions → Functions → Analytic, with `y` as the input argument, and then referenced directly in the Space Charge Density setting of the Electrostatics interface.

**Observations:**
- The potential profile along the center line showed a more complex, asymmetric curve compared to Case II — rising initially, peaking, then dipping into a minimum before rising again toward the second boundary condition.
- This asymmetric S-shaped behavior is consistent with a charge density that changes sign and magnitude linearly across the y-axis, since the corresponding potential is obtained by double-integrating a linear function, producing a cubic-like profile.
- The field arrows showed varying density and direction across the dielectric region, reflecting the spatially-dependent charge — strongest near the boundaries of the dielectric layer where |ρ(y)| was largest, and weaker near the center where the function approaches zero.
- This case demonstrated how COMSOL's Analytic Function tool allows space-dependent physical quantities to be defined symbolically rather than as fixed constants, which is essential for modeling realistic, non-uniform charge distributions found in many real dielectric materials.

## 8. Effect of Mesh Resolution

Across all three cases, mesh density was found to directly affect the smoothness and accuracy of the potential and field plots:

- **Coarse meshes** produced visibly jagged field arrows and a less smooth potential surface, particularly near the sharp corners of the electrodes where the field gradient is steepest.
- **Extra Fine and Extremely Fine meshes** produced smoother, more physically realistic field visualizations, at the cost of increased solving time.
- For Cases II and III, domain-based mesh refinement (applying a finer mesh specifically inside the charged region) provided a good balance — improving accuracy where the charge density varied most, without unnecessarily increasing the mesh density in the surrounding air domain.

## 9. Conclusion

This lab provided hands-on experience with the full COMSOL Multiphysics modeling workflow — geometry construction, domain selection, material assignment, physics configuration, meshing, solving, and postprocessing — applied to a classic electrostatics problem.

The progression from Case I to Case III demonstrated clearly how the introduction of space charge, first constant and then spatially varying, increasingly distorts the potential distribution away from the simple linear profile of an ideal capacitor. This reinforced the theoretical relationship between Poisson's equation and the resulting field behavior, and highlighted the importance of mesh resolution — particularly localized refinement — in accurately capturing steep gradients introduced by non-uniform charge distributions.

## 10. Files in This Repository

- `electrostatic-2D.mph` — COMSOL model file containing all three cases (Ideal, Uniform Charge, Non-Uniform Charge)
- `report.md` — this report

## 11. Tools and Software

- COMSOL Multiphysics 6.3 (AC/DC Module, Electrostatics interface)
- Class Kit License (CKL) — University of Kassel Legacy Cluster
