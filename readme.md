# The Analytics and Numerics of Sediment Transport in Rivers - Modeling Tidal Flow using the Shallow Water Equations 

## Domenico Lahaye and Henk Schuttelaars 

## Section 1: Models for Sediment Transport in Rivers

Model derivation for the shallow water equations. Starting from 3D laminar time-dependent Navier-Stokes (conservation of mass and momentum) arrive at depth integrated Navier-Stokes model. Integrate conservation of mass and momentum from bottom where $z=0$ to surface where $z = \xi(x,y,t)$. From conservation of mass, obtain $\int_{z=0}^{z=\xi} \nabla {\mathbh v} \, dz$. Use the boundary conditions no-slip at $z=0$, the kinematic boundary conditions that particle remains attached at the water surface at $z = \xi(x,y,t)$ and the fact that $\dot{z} = w(x,y,z,t)$. Use Leibniz formula integral of a derivative. Do similar for convervation of momentum. Finally obtain three conservation equations for longitudinal velocity component $u(x,y,t)$, transversal velocity component $v(x,y,t)$ and water height $h(x,y,t)$). 

Model derivation for coupling with sediment transport (scalar equation, describe from and to coupling Shallow Water Equations). 

### Pointers to literature on concepts 
- Shallow Water Equations [wiki](https://en.wikipedia.org/wiki/Shallow_water_equations)
- Bathymetry [wiki](https://en.wikipedia.org/wiki/Bathymetry)
- Coriolis_force [wiki](https://en.wikipedia.org/wiki/Coriolis_force) 
- Lecture series by Hillary Weller on the finite volume method for the shallow water equation [youtube](https://www.youtube.com/watch?v=1wWHqltukXo&t=46s)
- Sediment Transport [wiki](https://en.wikipedia.org/wiki/Sediment_transport)
- Bifurcation analysis [wiki](https://en.wikipedia.org/wiki/Bifurcation_theory) 

### Computational Domain

1. 1D line segment: linear and non-linear model. 
2. 2D rectangular channel with top and bottom wall, left inflow, right outflow. Inflow and outflow modeled by periodic boundary conditions. Analytical solution solution with uniform velocity profile avialable. Non-linear transport term drops out. 

## Section 2: The Numerics of the Harmonic Balance Method 

### Introduction: Wish - Dream - Goal - Ambition 

- numerics: classical approach discretization in space followed by discretization in time fails to see that problem is periodic 
- computer implementation: classical approach resort to sparse linear algebra that is hard to accelerate on GPU
- harmonic balance method: takes periodicity into account from the start and can be implementated using Fast Fourier Transforms (FFT) that are blazing fast on GPU. 

### Starting Humbly: Scalar ODE 

See [harmonic_balance wiki page](https://en.wikipedia.org/wiki/Harmonic_balance) for problem formulation and references to solution methods. See Reference nr. 8 on the same wiki page for more elaborate discussion. 

Equation $\ddot{x}(t) + x^3(t) = 0$. Observe no initial or boundary conditions are given. Observe that equation is non-linear and that therefore superposition does not apply.  

First assume solution of the form $x(t) = A \, \cos(\omega \, t)$ where $A$ and $\omega$ are constants to be determined. Then $\ddot{x}(t) = - A \, \omega^2 \, \cos(\omega \, t)$ and 
$x^3(t) = A^3 \, \cos^3(\omega \, t) = A^3 \, [ 3/4 \cos(\omega \, t) + 1/4 \cos(3\omega \, t)] $. Observe that the non-linearity gives raise to the third-order harmonic. Then equation becomes 
$ [ - A \, \omega^2 + 3/4 A^3] \cos(\omega t)+ 1/4 A^3 \cos(3\omega \, t) = 0$. Or after neglecting the third-order harmonic term, $[ - A \, \omega^2 + 3/4 A^3] \cos(\omega t) = 0 $, or $- A \, \omega^2 + 3/4 A^3$ or 
$\omega = 3/4 \sqrt{A}$. Observe how frequency $\omega$ and amplitude $A$ are linked by this equation. 

Second assume solution of the form $x(t) = A_1 \, \cos(\omega \, t) + A_3 \, \cos(3 \omega \, t)$ where $A_1$, $A_2$ and $\omega$ are constants to be determined.

What happens in case that damping (friction due to contact with surface) is introduced, i.e., $\ddot{x}(t) + \dot{x}(t) + x^3(t) = 0$ (expect solution to be no longer periodic). 

What happens in case that periodic forcing is introduced. $\ddot{x}(t) + \dot{x}(t) + x^3(t) = \sin(\omega_0 t)$. Do periodic solutions re-appear? 

Can we extend to more terms in the Fourier expansion? Does using [symbolics.jl](https://symbolics.juliasymbolics.org/stable/) help in solving these questions? 

### Mass-Spring-Damper System 

a/ Problem formuation: periodically forced mass-spring-damper with non-linear spring. Duffing equation.  Initially at rest. How does motion change with frequency of the driving force. Find equilibrium points and their kind. 

b/ Harmonic Balance or Fourier Spectral Method using HarmonicBalance.jl 

c/ Transient Simulation: using DifferentialEquations.jl 

d/ References: 
1. Duffing equation: [wiki](https://en.wikipedia.org/wiki/Duffing_equation#CITEREFJordanSmith2007): provides details of harmonic balance method for single harmonic.  
2. Harmonic balance method applied to the Duffing equation: [youtube link](https://www.youtube.com/watch?v=4gCx4_RWeS8) 
3. wiki on Harmonic Balance Method: [link](https://en.wikipedia.org/wiki/Harmonic_balance). 
4. book by Jordan and Smith: [link](https://www.google.nl/books/edition/Nonlinear_Ordinary_Differential_Equation/ewtREAAAQBAJ?hl=en&gbpv=1&dq=Jordan+smith+nonlinear+ordinary+differential+equations&printsec=frontcover) 
5. book Peter Deuflard 

### Scalar Linear PDE in 1D Space and Time 

Problem formulation: linear scalar wave equation with constant velocity $c_0$ given by $u_t(x,t) + c_0 u_x(x,t) = 0$ for $0 \leq x \leq 1$ and $0 \leq t \leq T$ supplied with periodic boundary conditions $u(0,t) = u(1,t)$ and initial conditions $u(x,t=0) = u_0(x)$.

Solve analytically: method of characteristics. Set $\xi = x - c_0 t$. Then $u_x = u_{\xi}$ and $u_t = - c_0 u_{\xi}$. Any function $u(\xi) = u(x - c_0 t)$ is solution. This function represents a profile (wave) moving from left to right.  

Solve using e.g. finite difference or finite volume method in space (uniform mesh is fine) followed by time integration applied to the semi-discrete (discrete in space and continuous in time) system. You can build time-integration routine from scratch or use DifferentialEquations.jl. Alternatively, use out-of-the box solutions such as [trixi.jl](https://trixi-framework.github.io). Expect numerical dispersion to occur unless some form of diffusion is added to the equations.  

Solve using a Fourier spectral method. See [spectral_method wiki page](https://en.wikipedia.org/wiki/Spectral_method). Assumme that $u(x,t) = c(t) \exp(i \, k \, x)$ (expansion in single harmonic with time dependent coefficient to be determined). Then $u_x = c(t) \, i \, k \, \exp(i \, k \, x)$ and $u_t = \dot{c}(t) \exp(i \, k \, x)$. This equation becomes $[ \dot{c}(t) - c_0 \, i \, k \, c(t) ]\exp(i \, k \, x) = 0$ or $\dot{c}(t) - c_0 \, i \, k \, c(t) = 0$ or $c(t) = A \, \exp( c_0 \, i \, k \, t)$ where $A$ is a constant of integration. Therefore $u(x,t) = A \, \exp(i \, k \, \xi)$. Expand to other initial conditions require more than only one harmonic mode. Add periodic excitation on the boundary. 


Repeat above for heat equation $u_t = u_{xx}$ plus boundary and initial conditions. 

Repeat above for convection-diffusion equation $u_t + c_0 u_x(x,t) = u_{xx}$ plus boundary and initial conditions.

### Scalar Non-Linear PDE in 1D Space and Time

Repeat above for convection-diffusion equation $u_t + u \, u_x(x,t) = u_{xx}$ plus boundary and initial conditions. The transport term is non-linear. 

Solve using convolution product from FourierTools.jl . 

### Shallow Water Equations in 1D 
												
a/ Problem formuation: 

b/ Fourier Spectral Method: using FourierFlow.jl in 1D space and time 

c/ Transient Simulation: using Trixi.jl in 1D space and time

d/ References 

wiki on Fourier spectral methods: https://en.wikipedia.org/wiki/Spectral_method
book Boyd on Fourier spectral methods 
ApproxFun.jl: solve 
SpectralKit.jl  
BSplineKit.jl 

### Shallow Water Equations in 2D Space and Time 

a/ Problem formuation: 

b/ Fourier Spectral Method: using FourierFlow.jl in 2D space and time

c/ Transient Simulation: using DifferentialEquations.jl in 2D space and time

d/ References 

### Coupling with Scalar Sediment Transport

## Section 3: The Julia Programming Language 

### Introductory material 

- Elementary introduction: [Thinking Julia](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html);
- Aalto Short Course: [julia-introduction](https://github.com/AaltoRSE/julia-introduction); 
- Video Collection by Chris Rackauckas: [link](https://www.youtube.com/playlist?list=PLCAl7tjCwWyGjdzOOnlbGnVNZk0kB8VSa) 
- Pointer to lots of goodies: [Nouvelles Julia](https://pnavaro.github.io/NouvellesJulia/pages/2022_03.html);

### Homebrewed by D. Lahaye 

- solving ODEs, including sytems obtained from spatial discretization of PDEs: [link to notebook](https://github.com/ziolai/ventura-modeling/blob/main/jupyter-notebooks/intro_ode.ipynb); 

### Shallow Water Equation (SWE) Solvers in Julia 

- Dedicated shallow water solver: [ShallowWaters.jl](https://github.com/milankl/ShallowWaters.jl) (can sediment transport (equation for C in Chapter 5 of [1]) be added as additional transport equation?)  
- As part of [Trixi](https://github.com/trixi-framework/Trixi.jl): [Shallow water eqn example in Trixi](https://github.com/trixi-framework/Trixi.jl/blob/main/examples/structured_2d_dgsem/elixir_shallowwater_source_terms.jl) (can sediment transport (equation for C in Chapter 5 of [1]) be added as additional transport equation?) 

### Bifurcation Analysis Tools in Julia 

Sediment transport in rivers has been documented to give raise to the formation of patterns in the sediment on the river bottom. These patterns are the equivalent to points of equilibrium of dynamical systems. Questions that arise in this context are: 
1. can the formation of patterns be computed efficiently?
2. can the stability of patterns (eigenvalues of local linear approximation) be analyzed efficiently? 
3. how does the type of stability (again eigenvalues of local linear approximation) change when parameters in the model (model for bed friction, magnitude and direction of the Coriolis force) change. 

This type of analysis is referred to as a bifurcation analysis. Dedicated tools for bifurcation analysis in Julia include: 
- Dedicated bifurcation analysis tool: [BifurcationKit.jl](https://github.com/bifurcationkit/BifurcationKit.jl)
- As part of [DifferentialEquations](https://diffeq.sciml.ai/stable/) : [Bifurcation Analysis](https://diffeq.sciml.ai/stable/analysis/bifurcation/) 

## References 

1. Master thesis of Marco Roozendaal: [link](https://repository.tudelft.nl/islandora/object/uuid%3Aedc2ffd6-00fd-4cd6-883b-13b14528cb72?collection=education) 
2. PhD Thesis of Tjebbe Hepkema: Chapter 5 in particular: [link](https://mega.nz/file/nMF2DaDA#W-nuZ_LKQkcN8x-dZiXY4VD1gNRiTzf46RH0RQCEP9E). Includes linear stability analysis. 
3. Description of code by Tjebbe Hepkema: [link](https://mega.nz/file/vJMmjZpA#8DapHLz7mGZsncUJTT2i3J4yukdMyz0oqmiUjXrsVbY)
4. PhD Thesis of Mirian Ter Brake: [link](https://repository.tudelft.nl/islandora/object/uuid:5cfcad13-0140-4ecc-a61b-217191b7611f?collection=research)
5. 2022 minor students report: [link](https://mega.nz/file/zMVySRYS#Pojfaiy0OrE1bncgTiRftYnuLzmDgiZM4t_xEneQSGQ)
6. 2022 minor students github repository: [link](https://github.com/victoriayuechen/Nonlinear-tidal-bars)  


```julia

```


```julia

```
