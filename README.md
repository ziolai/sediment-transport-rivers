# The Analytics and Numerics of Sediment Transport in Rivers 

## Domenico Lahaye and Henk Schuttelaars 

## Models for Sediment Transport in Rivers

### Pointers to literature on concepts 
- Shallow Water Equations [wiki](https://en.wikipedia.org/wiki/Shallow_water_equations)
- Sediment Transport [wiki](https://en.wikipedia.org/wiki/Sediment_transport)
- Bifurcation analysis [wiki](https://en.wikipedia.org/wiki/Bifurcation_theory) 

### Computational Domain

Rectangular channel with top and bottom wall, left inflow, right outflow. Inflow and outflow modeled by periodic boundary conditions. 

## Julia Programming Language 

- Elementary introduction: [Thinking Julia](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html);
- Aalto Short Course: [julia-introduction](https://github.com/AaltoRSE/julia-introduction); 
- Video Collection by Chris Rackauckas: [link](https://www.youtube.com/playlist?list=PLCAl7tjCwWyGjdzOOnlbGnVNZk0kB8VSa) 
- Pointer to lots of goodies: [Nouvelles Julia](https://pnavaro.github.io/NouvellesJulia/pages/2022_03.html); 

## Existing Components in Julia 

### Shallow Water Equation (SWE) Solvers 

- Dedicated shallow water solver: [ShallowWaters.jl](https://github.com/milankl/ShallowWaters.jl) (can sediment transport (equation for C in Chapter 5 of [1]) be added as additional transport equation?)  
- As part of [Trixi](https://github.com/trixi-framework/Trixi.jl): [Shallow water eqn example in Trixi](https://github.com/trixi-framework/Trixi.jl/blob/main/examples/structured_2d_dgsem/elixir_shallowwater_source_terms.jl) (can sediment transport (equation for C in Chapter 5 of [1]) be added as additional transport equation?) 

### Bifurcation Analysis 

To be applied to SWE + sediment after discretization in space. Parameter of interest is Coriolis force (magnitude, direction). 

- Dedicated bifurcation analysis tool: [BifurcationKit.jl](https://github.com/bifurcationkit/BifurcationKit.jl)
- As part of [DifferentialEquations](https://diffeq.sciml.ai/stable/) : [Bifurcation Analysis](https://diffeq.sciml.ai/stable/analysis/bifurcation/) 

## References 

1. PhD Thesis of Tjebbe Hepkema: Chapter 5 in particular: [link](https://mega.nz/file/nMF2DaDA#W-nuZ_LKQkcN8x-dZiXY4VD1gNRiTzf46RH0RQCEP9E)


```julia

```


```julia

```
