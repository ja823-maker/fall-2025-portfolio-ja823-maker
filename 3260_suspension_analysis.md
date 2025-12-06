---
layout: page
title: "Active Suspension Modeling & Control"
permalink: /active-suspension/
---

# MAE 3260 Final Project: Modeling and Control of an Active Suspension System  
**Team:** John Apessos, Usanbolor Amartuvshin, Charles Pearson

---

## 1. Introduction

This project focuses on modeling and controlling a **quarter-car active suspension system**.  
The system is analyzed using ODEs, state-space modeling, transfer functions, and closed-loop control design.

---

## 2. System Requirements & Assumptions

### Performance Requirements

- **Driver acceleration constraint**

$$ |a(t)| < 2\,\text{m/s}^2 $$

$$ |a(t)| < 1.5\,\text{m/s}^2 \quad \text{(active design)} $$

- **Driver displacement constraint**

$$ |y(t)| < 0.015\,\text{m} $$

- **Shock travel constraint**

$$ \text{travel} < 0.10\,\text{m} $$

### Quarter-Car Schematic

![schematic]({{ '/assets/images/suspension_schematic.png' | relative_url }})

---

## 3. State–Space Model

- **Governing ODEs**

$$ M_1 \ddot{x}_1 = b_1(\dot{x}_2 - \dot{x}_1) + k_1(x_2 - x_1) + f(t) $$

$$ M_2 \ddot{x}_2 = -b_1(\dot{x}_2 - \dot{x}_1) - k_1(x_2 - x_1) + k_2(d(t) - x_2) - f(t) $$

These represent force balance on the sprung and unsprung masses.

---

- **State vector**

$$ X = \begin{bmatrix} x_1 \\ \dot{x}_1 \\ x_2 \\ \dot{x}_2 \end{bmatrix} $$

- **Inputs**

$$ U = \begin{bmatrix} d(t) \\ f(t) \end{bmatrix} $$

- **Outputs**

$$ Y = \begin{bmatrix} x_1 \\ x_1 - x_2 \\ \dot{x}_1 \end{bmatrix} $$

---

## 4. Transfer Function Model

- **State-space representation**

$$ \dot{X} = AX + BU, \qquad Y = CX + DU $$

- **Laplace-domain state equation**

$$ X(s) = (sI - A)^{-1} B U(s) $$

- **Transfer function matrix**

$$ G(s) = C(sI - A)^{-1}B + D $$

- **System poles**

$$ \det(sI - A) = 0 $$

---

## 5. Closed-Loop Control

![block diagram]({{ '/assets/images/suspension_block.png' | relative_url }})

- **Closed-loop dynamics**

$$ \dot{X} = (A - B_f K C)X + B_d \, d(t) $$

### Tuned Simulation Parameters

| Parameter | Value |
|----------|-------|
| Sprung mass \(M_1\) | 300 kg |
| Unsprung mass \(M_2\) | 60 kg |
| Suspension stiffness \(k_1\) | 17,000 N/m |
| Suspension damping \(b_1\) | 2,500 N·s/m |
| Tire stiffness \(k_2\) | 190,000 N/m |
| Road step | 0.05 m |

---

## 6. Simulation Results (Passive vs Active)

![plots]({{ '/assets/images/suspension_plots.png' | relative_url }})

### Key Observations

- Active suspension reduces peak body displacement significantly.  
- Acceleration spike is dramatically lower.  
- Shock travel remains safely under 10 cm.  
- System returns to equilibrium faster under active control.  
- Frequency response shows reduced transmissibility at low frequencies.

---

## 7. Conclusion

The active suspension:

- Meets displacement, acceleration, and shock-travel constraints.  
- Improves comfort and stability.  
- Outperforms the passive suspension in both time and frequency domains.  

---

## 8. References

E. Alvarez-Sanchez, “A quarter-car suspension system: car body mass estimator and sliding mode control,” *Procedia Technology*, 2013.  
[https://www.researchgate.net/figure/A-quarter-car-model-of-suspension-system_fig1_270916660](https://www.researchgate.net/figure/A-quarter-car-model-of-suspension-system_fig1_270916660)

**MATLAB Code:**  
[https://drive.google.com/file/d/1xxntwCzga0kjwfw81-xwAA0x9ZLeVvZy/view?usp=sharing](https://drive.google.com/file/d/1xxntwCzga0kjwfw81-xwAA0x9ZLeVvZy/view?usp=sharing)

