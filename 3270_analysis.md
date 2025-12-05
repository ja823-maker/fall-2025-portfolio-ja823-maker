---
layout: page
title: "Torque Wrench Design Analysis"
permalink: /3270_analysis/
---

# MAE 3270: Instrumented Torque Wrench Design

This page documents my design, analysis, and finite element modeling work for the MAE 3270 **instrumented torque wrench** project. It tracks the progression from first-cut hand calculations through scripting, CAD modeling, and FEM validation.

---

## 1. Hand-Calculation Overview (First-Cut Design)

To develop an initial design for the torque-wrench handle, I model the handle as a **prismatic cantilever beam** of rectangular cross-section with thickness $$b$$ and width $$h$$. The handle is loaded by a tip force $$F$$ that produces the required torque $$T$$ about the drive:

$$
M = F L
$$

where $$L$$ is the distance from the drive to the load point and $$M$$ is the bending moment at the fixed section.

For a rectangular cross-section, the area moment of inertia and the distance from the neutral axis to the outer fiber are:

$$
I = \frac{b h^{3}}{12},
\qquad
c = \frac{h}{2}.
$$

The corresponding maximum bending stress and longitudinal strain at the outer surface (where the strain gauge is placed) are:

$$
\sigma = \frac{M c}{I},
\qquad
\varepsilon = \frac{\sigma}{E}.
$$

with $$E$$ the Young’s modulus of the chosen material.

The tip deflection is estimated using Euler–Bernoulli beam theory for a cantilever with an end load:

$$
\delta = \frac{F L^{3}}{3 E I}.
$$

These closed-form expressions provide a **first-cut design** for the wrench handle, giving nominal stresses, strains at the gauge location, and load-point deflection for any chosen set of dimensions $$(b, h, L)$$ and material properties $$(E, \sigma_y, S_{\text{fatigue}}, K_{IC})$$.

In the next step, I will expand this analysis by discussing the **material selection, applied loading conditions, and mechanical constraints** that define feasible geometries before moving into parametric design and finite-element modeling.

## 2. Material Selection and Design Constraints

To support the automated design search and later FEM validation, I first selected a material and defined the mechanical constraints governing feasible wrench-handle geometries. I chose **M42 high-speed steel**, a high-strength tool steel commonly used in load-bearing applications requiring toughness, fatigue resistance, and dimensional stability.

### 2.1 Material Properties (M42 Tool Steel)

| Property                                   | Symbol              | Value                             |
|-------------------------------------------|---------------------|-----------------------------------|
| Young’s modulus                           | $$E$$               | $$32 \times 10^6 \ \text{psi}$$   |
| Poisson’s ratio                           | $$\nu$$             | 0.29                              |
| Yield strength                            | $$S_y$$             | $$120 \ \text{ksi}$$              |
| Ultimate tensile strength                 | $$S_u$$             | $$370 \ \text{ksi}$$              |
| Fatigue strength at $$10^6$$ cycles       | $$S_{\text{fatigue}}$$ | $$115 \ \text{ksi}$$          |
| Plane-strain fracture toughness            | $$K_{IC}$$          | $$15 \ \text{ksi}\sqrt{\text{in}}$$ |

M42 provides high yield, ultimate, and fatigue strength, allowing a compact handle cross-section while maintaining acceptable safety factors. Its fracture toughness offers tolerance to small surface flaws or machining scratches, important for a tool that will undergo repeated torque cycling.

### 2.2 Design Loads

The instrumented torque-wrench handle is modeled as a cantilever beam of length $$L$$ loaded by a tip force $$F$$ that produces the required torque $$T$$ about the drive:

$$
M = F L,
\qquad
T = 600 \ \text{in·lbf}.
$$

For a handle length of $$L = 16 \ \text{in}$$, the corresponding applied force is

$$
F = \frac{T}{L}
  = \frac{600}{16}
  = 37.5 \ \text{lbf}.
$$

This load is applied at the free end in both the analytical calculations and the FEM model.

### 2.3 Performance Constraints for the Design Search

To ensure that the handle is structurally safe and produces a measurable strain-gauge output, the design variables $$(b, h)$$ and gauge location $$c$$ must satisfy:

- **Strain-gauge sensitivity requirement** at rated torque  
  $$
  \text{sensitivity} \ge 1.0 \ \text{mV/V at } T = 600 \ \text{in·lbf}.
  $$

- **Yield safety factor**  
  $$
  N_{\sigma} = \frac{S_y}{\sigma_{\text{max}}} \ge 4.
  $$

- **Fracture-toughness safety factor**  
  $$
  N_K = \frac{K_{IC}}{Y \, \sigma_{\text{max}} \sqrt{\pi a}} \ge 2,
  $$
  where $$Y$$ is the geometry factor and $$a$$ is the assumed surface-crack depth.

- **Fatigue safety factor** at $$10^6$$ cycles  
  $$
  N_s = \frac{S_{\text{fatigue}}}{\sigma_{\text{max}}} \ge 1.5.
  $$

- **Deflection constraint:** tip deflection must remain small so the wrench feels stiff and usable.

- The cross-section must provide **sufficient flat area** at distance $$c$$ for commercial strain-gauge bonding.

These material properties and constraints define the feasible design space used in the MATLAB design search. In the next section, I implement the beam-theory equations in a script that sweeps over candidate geometries $$(b, h, c)$$ and identifies the smallest cross-section that satisfies all required performance limits.

## 3. Automated Dimension Search (MATLAB Script)

To iterate the initial design, I wrote a MATLAB script that sweeps the handle height **h** while keeping the thickness **b = 0.50 in** and strain-gauge location **c = 1.0 in** fixed. For each candidate height, the script computes the nominal bending stress, strain, torque-wrench sensitivity (mV/V), and the safety factors for yield, fracture, and fatigue.

The search identifies the **smallest** height **h** that satisfies the design requirements outlined in **2.3**. The condensed MATLAB script used for this automated search is shown below:

![Automated search MATLAB script]({{ '/assets/images/wrench1.png' | relative_url }})

The script sweeps **h = 0.40–0.90 in**, applies standard Euler–Bernoulli beam formulas to compute nominal bending stress and deflection, evaluates sensitivity and safety factors for each candidate, and stops as soon as all requirements are met.

The script selected:

- **h = 0.612 in**  
- **b = 0.500 in**  
- **c = 1.000 in**  
- Sensitivity = **1.20 mV/V**  
- Yield SF = **6.24**  
- Fracture SF = **2.00**  
- Fatigue SF = **5.98**  
- Tip deflection = **0.17 in**

These results satisfy all constraints using *nominal* beam-theory stresses. In the FEM analysis that follows, local stresses near the drive block will increase due to stress concentrations, but this automated search provides a reasonable first-cut sizing of the handle geometry.

The next section uses these optimized dimensions to build the CAD model for the full finite-element analysis.

