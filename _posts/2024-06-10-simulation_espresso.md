---
title:  "Simulation of an espresso filter using the Brinkmann-Forchheimer equation"
search: true
categories: 
  - Simulation
last_modified_at: 2023-11-20
---

During my last semester in systems engineering I took the course "Advanced analysis and numerics" and me and two fellow students calculated the flow of water through an espresso filter in 2D.

**Read the full report in german [here] (/assets/pdf/HANA_Project.pdf)**

### Gleichungen: Brinkmann-Forchheimer Gleichung

$$\frac{1}{\varphi}\partial_t \boldsymbol{u} + \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} - \frac{\nu}{\varphi}\Delta\boldsymbol{u} +\frac{1}{\varrho} \nabla p + \frac{\nu}{K}\boldsymbol{u} + \frac{c_F}{\sqrt{K}}\vert \boldsymbol{u} \vert \boldsymbol{u} = 0$$

$$\nabla \cdot \boldsymbol{u} = 0$$
### Variablen und Konstanten
Abhängige Variablen:

$\boldsymbol{u}$: Strömungsgeschwindigkeit $\left[\frac{\text{m}}{\text{s}}\right]$ 
$p$: Druck $\left[\text{Pa} = \frac{\text{kg}}{\text{m}\cdot{s}^2}\right]$

Konstanten:
$\varphi$: Anteil Fluid $\in [0,1]$ $\left[-\right]$ 
$\nu$: Dynamische Viskosität $\left[\frac{\text{m}^2}{\text{s}}\right]$ $\left(\text{für Wasser @ 90°C} = 3.248 * 10^{-7}\:\frac{\text{m}^2}{\text{s}}\right)$ 
$\varrho$: Dichte $\left[\frac{\text{kg}}{\text{m}^3}\right]$ $\left(\text{für Wasser @ 90°C} = 965.31\:\frac{\text{kg}}{\text{m}^3}\right)$ 
$\text{K}$: Permeabilität $\left[\text{m}^2\right]$ 
$c_F$: Forchheimerkonstante $\left[-\right]$ 
$d_P$: Partikeldurchmesser $\left[\text{m}\right]$ 
$\alpha, \beta$: Formfaktoren (werden empirisch bestimmt, wir verwenden: $\alpha = 1.75, \beta = 150$)

Die Permeabilität $\text{K}$ ist definiert durch das Modell von Kozeny:

$$\text{K} = \frac{d_P^2\:\varphi^3}{\beta(1-\varphi)^2}$$
Die Forchheimer Konstante $c_F$ ist definiert durch:
$$c_F = \alpha \beta^{-1/2} \varphi^{-3/2}$$

### Randbedingungen

Der Fluss am Eingang des Siebs wird wie folgt berechnet:
$$ A = \frac{d^2*\pi}{4} = \frac{(0.04 \text{m})^2*\pi}{4} = 0.00126\text{m}^2 $$
$$u_{\text{in}} = \frac{V}{t\:A} = \frac{3e^{-5}\text{m}^3}{15\text{s}\cdot 0.00126 \text{m}^2} = 0.00159\: \text{m/s}$$

Die Randbedingungen für die Flussgeschwindigkeit sind gegeben durch
$$\partial \Omega = \Gamma_{\text{Wall}}+\Gamma_{\text{Inlet}}+ \Gamma_{\text{Outlet}}$$
$$\boldsymbol{u}(\boldsymbol{x}) = 0 \quad \text{on}\: \Gamma_{\text{Wall}}$$
$$\boldsymbol{u}(\boldsymbol{x}) = (0,-u_{\text{in}}) \quad \text{on}\: \Gamma_{\text{Inlet}}$$

die restlichen Randbedingungen werden bewusst offen gelassen.

### Schwache Gleichung

Zur Diskretisierung der Flussströmung $\boldsymbol{u}$ benutzen wir eine stetige H1-Vektor Diskretisierung 2. Ordnung mit zusätzlicher innerer Ordnung 3 und für den Druck eine diskontinuierliche Diskretisierung 1. Ordnung.
$$\boldsymbol{u}(\boldsymbol{x})\in V = P^{2+} \subset H^1(\Omega)$$
$$p(\boldsymbol{x}) \in Q = P^{1,dg} \subset H^1(\Omega)$$
Die starke Gleichung ist gegeben durch 

$$ \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} - \frac{\nu}{\varphi}\Delta\boldsymbol{u} +\frac{1}{\varrho} \nabla p + \frac{\nu}{K}\boldsymbol{u} + \frac{c_F}{\sqrt{K}}\vert \boldsymbol{u} \vert \boldsymbol{u} + \nabla \cdot \boldsymbol{u} = 0.$$
Wir vernachlässigen den zeitabhängigen Teil der Gleichung, da wir nur eine stationäre Lösung der Gleichung berechnen. 

Die einzelnen Komponenten der schwachen Gleichung berechnen sich wie folgt:

$$\begin{aligned} \frac{1}{\varphi^2}(\boldsymbol{u} \cdot \nabla)\boldsymbol{u} &= \frac{1}{\varphi^2}(\partial_x u_x + \partial_y u_y) \boldsymbol{u} = \frac{1}{\varphi^2} \begin{pmatrix} (\partial_x u_x + \partial_y u_y) u_x \\ (\partial_x u_x + \partial_y u_y) u_y \end{pmatrix} = \frac{1}{\varphi^2} \begin{pmatrix} \langle \nabla u_x, u \rangle  \\ \langle \nabla u_y, u \rangle \end{pmatrix} \\ &= \frac{1}{\varphi^2} \langle \nabla u, u \rangle \:\to \frac{1}{\varphi^2}\int_\Omega ((\nabla u)u)v \:d\boldsymbol{x}\quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned} $$

$$ 
\begin{aligned}\frac{\nu}{\varphi}\Delta\boldsymbol{u} &= \frac{\nu}{\varphi} \begin{pmatrix} \Delta u_x \\ \Delta u_y\end{pmatrix} \: \to \frac{\nu}{\varphi}\int_\Omega \begin{pmatrix} \Delta u_x \\ \Delta u_y\end{pmatrix} \cdot \begin{pmatrix}  v_x \\ v_y\end{pmatrix}\:d\boldsymbol{x} = \frac{\nu}{\varphi}\int_\Omega \begin{pmatrix} \nabla u_x \\ \nabla u_y\end{pmatrix} \cdot \begin{pmatrix}  \nabla v_x \\ \nabla v_y\end{pmatrix}\:d\boldsymbol{x} \\ & = \frac{\nu}{\varphi}\int_\Omega \langle \nabla u, \nabla v \rangle \:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned}$$

$$\begin{aligned} \frac{1}{\varrho} \nabla p\: \: \to \frac{1}{\varrho}\int_\Omega \begin{pmatrix} \partial_x p \\ \partial_y p\end{pmatrix} \cdot \begin{pmatrix}  v_x \\ v_y\end{pmatrix}\:d\boldsymbol{x} &= \frac{1}{\varrho}\int_\Omega \partial_x p \:v_x + \partial_y p \: v_y \:d\boldsymbol{x} = \frac{1}{\varrho}\int_\Omega (\partial_x \:v_x + \partial_y \: v_y)p \:d\boldsymbol{x} \\ &= \frac{1}{\varrho}\int_\Omega (\nabla \cdot v)p \:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) \end{aligned}$$

$$\frac{\nu}{K}\boldsymbol{u} \: \to \frac{\nu}{K}\int_\Omega u \: v\:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega) $$

$$\frac{c_F}{\sqrt{K}}\vert u \vert u \: \to \frac{c_F}{\sqrt{K}}\int_\Omega \vert u \vert u \: v\:d\boldsymbol{x} \quad \forall v \in V = P^{2+} \subset H^1(\Omega)$$

$$\nabla \cdot \boldsymbol{u}\: \to \int_\Omega (\nabla \cdot \boldsymbol{u})\:q \:d\boldsymbol{x}\quad \forall q \in Q = P^{1,dg} \subset H^1(\Omega)$$

Dies resultiert in der kompletten schwachen Gleichung

$$\begin{aligned} \int_\Omega \frac{1}{\varphi^2}((\nabla u)u)v + \frac{\nu}{\varphi} \langle \nabla u, \nabla v \rangle  + \frac{1}{\varrho}(\nabla \cdot v)p + \frac{\nu}{K} u \: v + \frac{c_F}{\sqrt{K}} \vert u \vert u \: v\ + (\nabla \cdot \boldsymbol{u})\:q \:d\boldsymbol{x} = 0 \\ \forall v \in V = P^{2+} \subset H^1(\Omega),\:\forall q \in Q = P^{1,dg} \subset H^1(\Omega) \end{aligned}$$

**Read the full report [here](/assets/pdf/ITK213_Final_Release.pdf)**


