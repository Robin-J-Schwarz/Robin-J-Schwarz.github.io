---
title:  "Simulation of an espresso filter using the Brinkmann-Forchheimer equation"
search: true
categories: 
  - Simulation
last_modified_at: 2023-11-20
---

During my last semester in systems engineering I took the course "Advanced analysis and numerics". Me and two class mates calculated the flow of water at 90°C through an espresso filter in 2D. 

**Read the full report in german [here](/assets/pdf/HANA_Project.pdf)**



### Brinkmann-Forchheimer Equation

$$\frac{1}{\varphi}\partial_t \boldsymbol{u} + \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} - \frac{\nu}{\varphi}\Delta\boldsymbol{u} +\frac{1}{\varrho} \nabla p + \frac{\nu}{K}\boldsymbol{u} + \frac{c_F}{\sqrt{K}}\vert \boldsymbol{u} \vert \boldsymbol{u} = 0$$

$$\nabla \cdot \boldsymbol{u} = 0$$

### Variables and Constants
Variables:

$$\boldsymbol{u}$$: Flowrate $$\left[\frac{\text{m}}{\text{s}}\right]$$ \\
$$p$$: Pressure $$\left[\text{Pa} = \frac{\text{kg}}{\text{m}\cdot{s}^2}\right]$$ \\

Constants:
$$\varphi$$: Fraction of Fluid $$4\in [0,1]$$ $$\left[-\right]$$ 
$$\nu$$: Dynamic Viscosity $$\left[\frac{\text{m}^2}{\text{s}}\right]$$ $$\left(\text{for Water @ 90°C} = 3.248 * 10^{-7}\:\frac{\text{m}^2}{\text{s}}\right)$$ 
$$\varrho$4: Density $$\left[\frac{\text{kg}}{\text{m}^3}\right]$ $\left(\text{for Water @ 90°C} = 965.31\:\frac{\text{kg}}{\text{m}^3}\right)$$
$$\text{K}$$: Permeability $$\left[\text{m}^2\right]$$ 
$$c_F$$: Forchheimerconstant $$\left[-\right]$$ 
$$d_P$$: Particle diameter $$\left[\text{m}\right]$$ 
$$\alpha, \beta$$: Form factors (are determined empirically, we use: $$\alpha = 1.75, \beta = 150$$)

### Boundary conditions

The flow at the inlet of the filter is calculated as follows:
$$ A = \frac{d^2*\pi}{4} = \frac{(0.04 \text{m})^2*\pi}{4} = 0.00126\text{m}^2 $$
$$u_{\text{in}} = \frac{V}{t\:A} = \frac{3e^{-5}\text{m}^3}{15\text{s}\cdot 0.00126 \text{m}^2} = 0.00159\: \text{m/s}$$

The boundary conditions for the flow velocity are given by
$$\partial \Omega = \Gamma_{\text{Wall}}+\Gamma_{\text{Inlet}}+ \Gamma_{\text{Outlet}}$$
$$\boldsymbol{u}(\boldsymbol{x}) = 0 \quad \text{on}\: \Gamma_{\text{Wall}}$$
$$\boldsymbol{u}(\boldsymbol{x}) = (0,-u_{\text{in}}) \quad \text{on}\: \Gamma_{\text{Inlet}}$$

and the remaining boundary conditions are deliberately left open.

### Weak Equation

For the discretisation of the flow $$\boldsymbol{u}$$ we use a continuous 2nd order H1-vector discretisation with additional inner order 3 and for the pressure a discontinuous 1st order discretisation..
$$\boldsymbol{u}(\boldsymbol{x})\in V = P^{2+} \subset H^1(\Omega)$$
$$p(\boldsymbol{x}) \in Q = P^{1,dg} \subset H^1(\Omega)$$

The strong equation is given by: 
$$ \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} - \frac{\nu}{\varphi}\Delta\boldsymbol{u} +\frac{1}{\varrho} \nabla p + \frac{\nu}{K}\boldsymbol{u} + \frac{c_F}{\sqrt{K}}\vert \boldsymbol{u} \vert \boldsymbol{u} + \nabla \cdot \boldsymbol{u} = 0.$$
We neglect the time-dependent part of the equation, as we only calculate a stationary solution to the equation. 

The individual components of the weak equation are calculated as follows:

$$\begin{aligned} \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} &= \frac{1}{\varphi^2}(\partial_x u_x + \partial_y u_y) \boldsymbol{u} = \frac{1}{\varphi^2} \begin{pmatrix} (\partial_x u_x + \partial_y u_y) u_x \\ (\partial_x u_x + \partial_y u_y) u_y \end{pmatrix} = \frac{1}{\varphi^2} \begin{pmatrix} \langle \nabla u_x, u \rangle  \\ \langle \nabla u_y, u \rangle \end{pmatrix} \\ &= \frac{1}{\varphi^2} \langle \nabla u, u \rangle \:\to \frac{1}{\varphi^2}\int_\Omega ((\nabla u)u)v \:d\boldsymbol{x}\quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned} $$

$$ 
\begin{aligned}\frac{\nu}{\varphi}\Delta\boldsymbol{u} &= \frac{\nu}{\varphi} \begin{pmatrix} \Delta u_x \\ \Delta u_y\end{pmatrix} \: \to \frac{\nu}{\varphi}\int_\Omega \begin{pmatrix} \Delta u_x \\ \Delta u_y\end{pmatrix} \cdot \begin{pmatrix}  v_x \\ v_y\end{pmatrix}\:d\boldsymbol{x} = \frac{\nu}{\varphi}\int_\Omega \begin{pmatrix} \nabla u_x \\ \nabla u_y\end{pmatrix} \cdot \begin{pmatrix}  \nabla v_x \\ \nabla v_y\end{pmatrix}\:d\boldsymbol{x} \\ & = \frac{\nu}{\varphi}\int_\Omega \langle \nabla u, \nabla v \rangle \:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned}$$

$$\begin{aligned} \frac{1}{\varrho} \nabla p\: \: \to \frac{1}{\varrho}\int_\Omega \begin{pmatrix} \partial_x p \\ \partial_y p\end{pmatrix} \cdot \begin{pmatrix}  v_x \\ v_y\end{pmatrix}\:d\boldsymbol{x} &= \frac{1}{\varrho}\int_\Omega \partial_x p \:v_x + \partial_y p \: v_y \:d\boldsymbol{x} = \frac{1}{\varrho}\int_\Omega (\partial_x \:v_x + \partial_y \: v_y)p \:d\boldsymbol{x} \\ &= \frac{1}{\varrho}\int_\Omega (\nabla \cdot v)p \:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned}$$

$$\frac{\nu}{K}\boldsymbol{u} \: \to \frac{\nu}{K}\int_\Omega u \: v\:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) $$

$$\frac{c_F}{\sqrt{K}}\vert u \vert u \: \to \frac{c_F}{\sqrt{K}}\int_\Omega \vert u \vert u \: v\:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega)$$

$$\nabla \cdot \boldsymbol{u}\: \to \int_\Omega (\nabla \cdot \boldsymbol{u})\:q \:d\boldsymbol{x}\quad \forall q \in Q = P^{1,dg} \subset H^1(\Omega)$$

This results in the complete weak equation

$$\begin{aligned} \int_\Omega \frac{1}{\varphi^2}((\nabla u)u)v + \frac{\nu}{\varphi} \langle \nabla u, \nabla v \rangle  + \frac{1}{\varrho}(\nabla \cdot v)p + \frac{\nu}{K} u \: v + \frac{c_F}{\sqrt{K}} \vert u \vert u \: v\ + (\nabla \cdot \boldsymbol{u})\:q \:d\boldsymbol{x} = 0 \\ \forall v \in V = P^{2+} \subset H^1(\Omega),\:\forall q \in Q = P^{1,dg} \subset H^1(\Omega) \end{aligned}$$

### Solution




