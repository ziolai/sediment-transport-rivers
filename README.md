# The Analytics and Numerics of Sediment Transport in Rivers - Modeling Tidal Flow using the Shallow Water Equations 

## Domenico Lahaye and Henk Schuttelaars 

## Section 1: Models for Sediment Transport in Rivers

Model derivation for shallow water equtions (depth integrated Navier-Stokes), boundary conditions and initial conditions (three coupled equations, two vertical velocity components $u(x,y,t)$ and $v(x,y,t)$ and water height $h(x,y,t)$). 

Model derivation for coupling with sediment transport (scalar equation, describe from and to coupling Shallow Water Equations). 

### Pointers to literature on concepts 
- Shallow Water Equations [wiki](https://en.wikipedia.org/wiki/Shallow_water_equations)
- Sediment Transport [wiki](https://en.wikipedia.org/wiki/Sediment_transport)
- Bifurcation analysis [wiki](https://en.wikipedia.org/wiki/Bifurcation_theory) 

### Computational Domain

1. 1D line segment 
2. 2D rectangular channel with top and bottom wall, left inflow, right outflow. Inflow and outflow modeled by periodic boundary conditions. 

## Section 2: The Numerics of the Harmonic Balance Method 

### Introduction: Wish - Dream - Goal - Ambition 

- numerics: classical approach discretization in space followed by discretization in time fails to see that problem is periodic 
- computer implementation: classical approach resort to sparse linear algebra that is hard to accelerate on GPU
- harmonic balance method: takes periodicity into account from the start and can be implementated using Fast Fourier Transforms (FFT) that are blazing fast on GPU. 

### Starting Humbly: Scalar ODE 

[link](https://en.wikipedia.org/wiki/Harmonic_balance)

See Reference nr. 8 on the wiki page? 

Can we extend to more terms in the Fourier expansion? Does using symbolics.jl help in this? 

### Mass-Spring-Damper System 

a/ Problem formuation: periodically forced mass-spring-damper with non-linear spring. Initially at rest. How does motion change with frequency of the driving force. Find equilibrium points and their kind. 

b/ Harmonic Balance or Fourier Spectral Method using HarmonicBalance.jl 

c/ Transient Simulation: using DifferentialEquations.jl 

d/ References 
wiki on Harmonic Balance Method: [link](https://en.wikipedia.org/wiki/Harmonic_balance). 
book Peter Deuflard 

### Scalar Linear PDE in 1D Space and Time 

Solve linear scalar wave u_t - c u_x = 0. See [link](https://en.wikipedia.org/wiki/Spectral_method). Leads to u_t = (ik) u(t) yielding as solution u(t) = exp(jk t). 
Extend to heat equation using seperation of variables. 
Extend to  heat equation plus convective term.  

### Scalar Non-Linear PDE in 1D Space and Time

Turn convective term into non-linear term. Solve using convolution product from FourierTools.jl . 

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

1. PhD Thesis of Tjebbe Hepkema: Chapter 5 in particular: [link](https://mega.nz/file/nMF2DaDA#W-nuZ_LKQkcN8x-dZiXY4VD1gNRiTzf46RH0RQCEP9E)
2. Description of code by Tjebbe Hepkema: [link](https://mega.nz/file/vJMmjZpA#8DapHLz7mGZsncUJTT2i3J4yukdMyz0oqmiUjXrsVbY)
3. PhD Thesis of Mirian Ter Brake: [link](https://repository.tudelft.nl/islandora/object/uuid:5cfcad13-0140-4ecc-a61b-217191b7611f?collection=research)
4. 2022 minor student report: [link](https://mega.nz/file/zMVySRYS#Pojfaiy0OrE1bncgTiRftYnuLzmDgiZM4t_xEneQSGQ)


```julia

```


```julia

```
