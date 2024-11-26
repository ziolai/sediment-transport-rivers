# Riding Shallow Waters 

<div>
<img src="./riding-shallow-waters.png" width=600 /> 
</div>


## with Domenico Lahaye and Henk Schuttelaars 

## Section 1: Introduction 

This project aims at contributing to the computational modeling of [tidal flows](https://en.wikipedia.org/wiki/Tide#Current) and [sediment transport](https://en.wikipedia.org/wiki/Sediment_transport) in rivers. The flow of water in rivers can be described by the [shallow water equations](https://en.wikipedia.org/wiki/Shallow_water_equations) (linear vs. non-linear variant, laminar vs. turbulent model). When exicited periodically (e.g. by tidal motion at the inlet of the channel), the non-linear nature of the equations will deform (modulate) the amplitude and frequency of the driving system. After sufficiently long time, the signal will become periodic again. Both time-integration (after spatial discretization or method of lines) and harmonic balance methods (either after spatial or temporal discretization) will be explored. See the Section entitled Analysis in [wikipedia page on tidal flows](https://en.wikipedia.org/wiki/Tide#Current) for a motivation of harmonic balance method in the context of this project. 

The <b>goals</b> of the project include 
1. to solve the shallow water equations using a blend of analytical and numerical methods;   
2. to compute the amplitude and temporal frequency content of the computed axial and transversal velocity components and the water height;
3. to discover patterns in the sediment formation and study the stability of these patterns (via bifurcation analysis).

The <b>use of the Julia programming language</b> is an integral part of the learning objectives of this project. Non-linear terms play an essential role in modifying the temporal frequency content of waves as they propagate. The analysis of these non-linear terms requires the computation of the Jacobian, independent of whether a transient time-stepping or harmonic balance method is used. Functions to compute these Jacobians in Python do exist. These functions, however, are either computationally costly (in case that finite difference quotients are used) or non-trivial to use (in case that automatic differentiation in a library like e.g. [JAX](https://jax.readthedocs.io/en/latest/quickstart.html) is used). Switching to Julia alleviates these bottlenecks. 
 
## Section 2: Project Levels 

The project is divided in various levels that are outlined below.

### Beginner Level: Mass-Spring-Damper Systems 

Here we lump the body of water to a point-mass. We assume this body to be subject to periodic forcing (alternating low and high tide) and friction (due to e.g. contact with river bed). The effect of non-linear springs and non-linear dampers on the motion of the mass subject to periodic forcing will be explored. The goal of this level to introduce key concepts, analysis tools and software techniques in the project.

The primary notebook for this level is [notebook on mass-spring-damper systems](./mass-spring-damper-systems.ipynb)

Supporting notebooks for this level include
1. notebook on [the harmonic balance method](./harmonic_balance_method.ipynb);
2. notebook on [Time-Integration using DifferentialEquations.jl](https://github.com/ziolai/software/blob/master/intro_ode.ipynb)
3. notebook on [analytical and symbolic computations](./analytical_symbolic_computations.ipynb);  

The <b>goals</b> of beginners level of the assignments are to: 
1. solve periodic solutions (equilibrium between forcing, damping and non-linear stiffness) of the Duffing equation using <b>time-integration</b>. Investigate aspects such as computational cost (CPU-time and memory requirements) vs. accuracy (time step size, absolute tolerance, relative tolerance), implicit vs. explicit time integration methods, low vs. high order time integration methods, second order vs. coupled first order formulation, operator splitting allowing to treat linear stiffness implicitly and non-linar stiffness explicitly, automatic differentiation to compute the Jacobian and GPU acceleration;
2. solve periodic solution using the <b>harmonic balance method</b>. As above. Include number of harmonics in the harmonic balance soliution;' 
3. investigate how periodic solutions change with <b>changing parameters</b> (stiffness, damping, amplitude and frequency of the excitation); 

In case successful, possibly extend to multiple interconnected point masses. 

### Intermediate Level: Non-Linear Scalar Wave Equation 

Here we describe water wave as transversal or longitudinal wave propagation in spring or membrane excited by periodic forcing. The physics of the change of water height is neglected. The effect of the non-linearity on the temporal frequency content of the solution will again be explored. 

The primary notebook for this level is [notebook on scalar wave equations](./scalar-wave-equation.ipynb) (required extension from 1D space to 2D space and implementation of periodic boundary conditions). 

Supporting notebooks
1. seperation of variables to solve linear wave equation analytically taking various boundary conditions into account; 
2. method of lines to linear wave equation numerically by first discretizing in space and subsequently use time integration; 


### Expert Level: Tidal Flow in Rivers and the Shallow Water Equations 

Shallow water equations describe the propagation of water waves in rivers. Both linear and non-linear variants of the model. They are derived from the Navier-Stokes equations by averaging in the depth direction. 

The primary notebook for this level is [notebook on shallow water equations](./shallow-water-equations.ipynb) (requires implementation of linear SWE (operators constant throught time integration) and non-linear SWE (operators updated at each time step)). 

### Expert+ Level: Pattern Formation in Sediment Transport in Rivers 

Add coupling of the shallow water equation with additional transport (convection-diffusion) equation for concentration of sediment in water. Describe two-way coupling between water flow and sediment transport. Water flow transports the sediment. The sediment add mass and viscosity to the water and slows down the water.    

Add bifurcation analysis to study points of equilibrium of the dynamical system (roots of coupled system of algebraic equations) and their stability (spectrum of the Jacobian at the point of equilibrium). 

<b>To add:</b> Add demo using scalar wave equation with non-linear damping (with animation of both the excitation and the solution with both non-linear damping or non-linear stiffness) would be good to have here).

## Section 3: Introductory material on the Julia Programming Language

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
