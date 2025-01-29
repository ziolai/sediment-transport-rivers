# Riding Shallow Waters 

<div>
<img src="./antwerp-port.png" width=600 /> 
</div>


## with Domenico Lahaye and Henk Schuttelaars 

### To do 

1. Duffing equation: compare transient and HB: first for linear prblem. Perform FFT after transient simulation for various values of the driving frequency. Extract sine and cos mode amplide and compare with HB method; 
1. Duffing equation HB single harmonic: solve by classical Newton. Inial guess seems to matter. Should we iterate in reverse order? Do we need to supply a Jacobian? Do we need to look into the residual norm? Does solver provide line-searches or trust-region? Do we need to plot the landscape? 
1. Duffing equation HB single harmonic: do we learn from [this page](https://euphonics.org/8-2-2-duffings-equation-and-harmonic-balance/). 
1. Duffing equation HB single harmonic: see this page [this python solver](https://josephcslater.github.io/mousai/tutorial/demos/Theory_and_Examples.html): interesting perspactive on comparing the harmonic balance method with time-intergration: shows how the unstable branch for solution is not obtained (or reached) using time integration? 
1. Duffing equation HB single harmonic: solve by a continuation method (either HarmonicBalance.jl or BifurcationKit.jl) and show that the latter results in more solutions; 
1. extend above to non-linear case with small excitation so that only resonant frequency changes;
1. extend above to non-linear case with large excitation so that second harmonic appears; 
1. single harmonic Duffing to double harmonic Duffing. Stuck in the polynomial expansion;
1. wave equation: spectral analysis of A and AA matrices. Stuck in computing eigenvalues of AA; 
1. continue writng the [discourse post](discourse-post.ipynb) 

## Section 1: Introduction 

This project aims at contributing to the computational modeling of [tidal flows](https://en.wikipedia.org/wiki/Tide#Current) and [sediment transport](https://en.wikipedia.org/wiki/Sediment_transport) in rivers. The flow of water in rivers can be described by the [shallow water equations](https://en.wikipedia.org/wiki/Shallow_water_equations) (linear vs. non-linear variant, laminar vs. turbulent model). When exicited periodically (e.g. by tidal motion at the inlet of the channel), the non-linear nature of the equations will deform (modulate) the amplitude and frequency of the driving system. After sufficiently long time, the signal will become periodic again. Both time-integration (after spatial discretization or method of lines) and harmonic balance methods (either after spatial or temporal discretization) will be explored. See the Section entitled Analysis in [wikipedia page on tidal flows](https://en.wikipedia.org/wiki/Tide#Current) for a motivation of harmonic balance method in the context of this project. 

The <b>goals</b> of the project include 
1. to solve the shallow water equations using a blend of analytical and numerical methods;   
2. to compute the amplitude and temporal frequency content of the computed axial and transversal velocity components and the water height;
3. to discover patterns in the sediment formation and study the stability of these patterns (via bifurcation analysis).

<b>Socio-economical impact of this research</b> 
Protect and save-guard infrastructure. Maintenance of passage ways in harbour (Antwerp as example). 

The <b>use of the Julia programming language</b> is an integral part of the learning objectives of this project. Non-linear terms play an essential role in modifying the temporal frequency content of waves as they propagate. The analysis of these non-linear terms requires the computation of the Jacobian, independent of whether a transient time-stepping or harmonic balance method is used. Functions to compute these Jacobians in Python do exist. These functions, however, are either computationally costly (in case that finite difference quotients are used) or non-trivial to use (in case that automatic differentiation in a library like e.g. [JAX](https://jax.readthedocs.io/en/latest/quickstart.html) is used). Switching to Julia alleviates these bottlenecks. 
 
## Section 2: Harmonic Balance Method 

The harmonic balance method is explained in the notebook on [the harmonic balance method](./harmonic_balance_method.ipynb).

## Section 3: Project Levels 

The project is divided in various levels that are outlined below.

### Section 1.3: Beginner Level: Mass-Spring-Damper Systems 

Here we lump the body of water to a single point-mass or a system of multiple point masses. We assume this body to be subject to periodic forcing (alternating low and high tide) and friction (due to e.g. contact with river bed). The effect of non-linear springs and non-linear dampers on the motion of the mass subject to periodic forcing will be explored. The goal of this level to introduce key concepts, analysis tools and software techniques in the project.

The primary notebook for this level is [notebook on mass-spring-damper systems](./mass-spring-damper-systems.ipynb)

Supporting notebooks for this level include 
1. notebook on [time-integration using DifferentialEquations.jl](https://github.com/ziolai/software/blob/master/intro_ode.ipynb)
1. notebook on [analytical computations](./analytical_computations.ipynb).
1. notebook on [symbolic computations in Python](./symbolic_python.ipynb). Symbolic computations in this notebook are performed using [sympy](https://www.sympy.org/en/index.html);
1. notebook on [symbolic computations using Symbolics.jl](./symbolic_julia.ipynb). Symbolic computations in this notebook are performed using [Symbolics.jl](https://docs.sciml.ai/Symbolics/stable/) and [SymbolicUtils.jl](https://symbolicutils.juliasymbolics.org). We derive the non-linear system of equations for the ampliutudes of the cosine and sine mode. 
1. notebook on [solving the harmonic balance equations](./bifurcationkit.ipynb). In this notebook the harmonic balance equations are solved using bifurcation analysis tools. Both non-linear damping and non-linear stiffness are introduced;   

The <b>goals</b> of beginners level of the assignments are to: 
1. solve for periodic solutions (equilibrium between forcing, damping and non-linear stiffness) of the [Duffing equation](https://en.wikipedia.org/wiki/Duffing_equation) using <b>time-integration</b>. Investigate aspects such as computational cost (CPU-time and memory requirements) vs. accuracy (time step size, absolute tolerance, relative tolerance), implicit vs. explicit time integration methods, low vs. high order time integration methods, second order vs. coupled first order formulation, operator splitting allowing to treat linear stiffness implicitly and non-linar stiffness explicitly, automatic differentiation to compute the Jacobian and GPU acceleration;
2. solve for periodic solution using the <b>harmonic balance method</b>. As above. Include number of harmonics in the harmonic balance soliution;' 
3. investigate how periodic solutions change with <b>changing parameters</b> (stiffness, damping, amplitude and frequency of the excitation) using bifurcation analysis; 

In case successful, possibly extend to multiple interconnected point masses (coupled Duffing oscillators, cite as appropriate). 

<b>To elaborate further</b>
1. (later): transient analysis: two-mass system: coupled position - velocity formulation: [IMEX solver](https://docs.sciml.ai/DiffEqDocs/stable/solvers/split_ode_solve/#Implicit-Explicit-(IMEX)-ODE): linear solver setup prior to time-stepping loop; 
1. (later): harmonic analysis: preconditioned Krylov subspace solver for the Jacobian at each Newton step; structure of the Jacobian; large number of small blocks vs. small number of large blocks; block ILU after reordering of the coefficient matrix; what Krylov package to use? 

### Section 2.3: Intermediate Level: Non-Linear Scalar Wave Equation 

Here we describe water waves as transversal or longitudinal wave propagation in spring or membrane excited by periodic forcing. The physics of the change of water height is neglected. For the numerical solutions, we first discretize the eqiuations in space and subequently solve the resulting system of ordinary differential equations using a time-integration method. We thus apply the method of lines to solve the partial differential equation numerically. For simplicity, we apply for spatial discretization the finite difference method on a uniform mesh. The effect of the non-linearity on the temporal frequency content of the solution will again be explored. 

The primary notebook for this level is [non-linear 1D scalar wave equation](./nonlinear-wave-equation.ipynb).

Supporting notebooks for this level include:
1. [notebook on 1D scalar wave equations](./scalar-wave-equation.ipynb)
1. information on seperation of variables, eigenvalues and eigenmodes [seperation-variables](seperation-variables.ipynb). Of interst in discovering resonant modes in frequency response sweeps; 
1. preliminary results of time-integration of the two-dimensional wave equation using finite differences on  uniform mesh in the internship of Anouchka Desmettre [internship previous student](https://github.com/AnouchkaDESMETTRE/TU_Delft_Internship);  
1. wave equation with cubic damping solved by a harmonic balance method with two modes leading to two coupled Helmholtz equations for the amplitudes $A(x)$ and $B(x)$. This coupled system can be solved by a shooting method. [nonlinear-wave-equation](nonlinear-wave-equation.ipynb)  

#### One-Dimensional Space-Time Linear Wave Equation with Periodic Forcing

1. problem formulation: PDE plus periodic boundary conditions and initial conditions for position and velocity. Possibly incluce a linear transport term (using first order upwinding); 
2. analytical reference solutions (at least for the linear case) using separartion of variables taking various types of boundary conditions into acount. Possibly use symbolic computations; 
3. spatial discretization using second-order central finite difference scheme on a uniform mesh; 
4. time discretization in both second order and coupled first order;
5. harmonic balance method (linear problem formulation, problem set-up and problem solve for single and multiple frequencies); 
6. bifurcation analysis for periodic-in-time solution and analysis for changing parameters; 

#### One-Dimensional Space-Time Non-Linear Wave Equation with Periodic Forcing

1. Extend above with non-linear damping and transport term;  

#### Two-Dimensional Space-Time (Non-)Linear Wave Equation with Periodic Forcing

1. Extend above from 1D space (only $x$) to 2D space (both $x$ and $y$) by tensor product;  

<b>To elaborate further</b>
1. bifurcation analysis: bifurcationkit.jl on the scalar wave equation after transformation to the harmonic balance mthod  
1. transient analysis: IMEX 
1. harmonic analysis: preconditioned Krylov method within the Newton iteration; 

###  Section 3.3: Expert Level: Tidal Flow in Rivers and the Shallow Water Equations 

Shallow water equations describe the propagation of water waves in rivers. Both linear and non-linear variants of the model. They are derived from the Navier-Stokes equations by averaging in the depth direction. 

The primary notebook for this level is [notebook on shallow water equations](./notes-shallow-water-equations.ipynb) (requires implementation of linear SWE (operators constant throught time integration) and non-linear SWE (operators updated at each time step)). 

Supporting notebooks for this level include
1. notebook on [scalar advection equation](./scalar_advection-equation.ipynb) (here we exclude diffusion. Wave propagation in one direction only);
1. notebook on [one-dimensional shallow water equations](./one-dim-shallow-water-equations.ipynb) (coupled system of two transport equations); 
1. notebook on [two-dimensional shallow water equations](./one-dim-shallow-water-equations.ipynb);

###  Section 4.3: Expert+ Level: Pattern Formation in Sediment Transport in Rivers 

Add coupling of the shallow water equation with additional transport (convection-diffusion) equation for concentration of sediment in water. Describe two-way coupling between water flow and sediment transport. Water flow transports the sediment. The sediment add mass and viscosity to the water and slows down the water.    

Add bifurcation analysis to study points of equilibrium of the dynamical system (roots of coupled system of algebraic equations) and their stability (spectrum of the Jacobian at the point of equilibrium). 

## Section 4: Introductory material on the Julia Programming Language

- Elementary introduction: [Thinking Julia](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html);
- Aalto Short Course: [julia-introduction](https://github.com/AaltoRSE/julia-introduction); 
- Video Collection by Chris Rackauckas: [link](https://www.youtube.com/playlist?list=PLCAl7tjCwWyGjdzOOnlbGnVNZk0kB8VSa) 
- Pointer to lots of goodies: [Nouvelles Julia](https://pnavaro.github.io/NouvellesJulia/pages/2022_03.html);

### Bifurcation Analysis Tools in Julia (Expert+ Level)

Sediment transport in rivers has been documented to give raise to the formation of patterns in the sediment on the river bottom. These patterns are the equivalent to points of equilibrium of dynamical systems. Questions that arise in this context are: 
1. can the formation of patterns be computed efficiently?
2. can the stability of patterns (eigenvalues of local linear approximation) be analyzed efficiently? 
3. how does the type of stability (again eigenvalues of local linear approximation) change when parameters in the model (model for bed friction, magnitude and direction of the Coriolis force) change. 

This type of analysis is referred to as a bifurcation analysis. Dedicated tools for bifurcation analysis in Julia include: 
- Dedicated bifurcation analysis tool: [BifurcationKit.jl](https://github.com/bifurcationkit/BifurcationKit.jl)
- As part of [DifferentialEquations](https://diffeq.sciml.ai/stable/) : [Bifurcation Analysis](https://diffeq.sciml.ai/stable/analysis/bifurcation/) 

## References 

1. Book by Malte Krack and Johann Gross <i>Harmonic Balance for Non-Linear Vibration Problems</i>: [link](https://mega.nz/file/fYFWxQBT#OzIjwMd56nQDBzOeJ1VdSAWIO6i3dWuzUw4qnsFCQHs); 
2. Master thesis of Marco Roozendaal: [link](https://repository.tudelft.nl/islandora/object/uuid%3Aedc2ffd6-00fd-4cd6-883b-13b14528cb72?collection=education) 
3. PhD Thesis of Tjebbe Hepkema: Chapter 5 in particular: [link](https://mega.nz/file/nMF2DaDA#W-nuZ_LKQkcN8x-dZiXY4VD1gNRiTzf46RH0RQCEP9E). Includes linear stability analysis. 
4. PhD Thesis of Mirian Ter Brake: [link](https://repository.tudelft.nl/islandora/object/uuid:5cfcad13-0140-4ecc-a61b-217191b7611f?collection=research)
5. 2022 minor students report: [link](https://mega.nz/file/zMVySRYS#Pojfaiy0OrE1bncgTiRftYnuLzmDgiZM4t_xEneQSGQ)
6. 2022 minor students github repository: [link](https://github.com/victoriayuechen/Nonlinear-tidal-bars)
7. 2023 minor students report part A: [link](https://mega.nz/file/CMkn3KaK#F0VkA8qduoqYQhuUdhllTsHJSetERdP6oF2yeszb7gg)
8. 2023 minor students report part B: [link](https://mega.nz/file/LMUElTzK#phkzaRSQ2uu-eSgxM-pS2zGq7ASQ96GhOG1myYRJabg)
9. 2023 minor students report part B further elaborated: [link](https://mega.nz/file/bcVilJSb#l7eQk-9NWH-ICmZ3u886W8vn3Ds7jheNu6q4xqjSkZs)
10. Chapter 7 on forced oscillations of the book Nonlinear Ordinary Differential Equation by Jordan and Smith discusses forced oscillations: see [link](https://www.google.nl/books/edition/Nonlinear_Ordinary_Differential_Equation/ewtREAAAQBAJ?hl=en&gbpv=1&dq=Jordan+smith+nonlinear+ordinary+differential+equations&printsec=frontcover)
11. paper by Elliot e.a. Nonlinear damping and quasi-linear modeling provides reference solutions for the harmonic balence method;


```julia

```
