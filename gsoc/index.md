---
layout: gsocblog
title: GSoC blog
---

# Overview (Updated)

- [x] Implementation for evaluating `FEFunction` on arbitrary
      points. This covers both Lagrange and RT elements.

- [x] Extending `interpolate_everywhere(::FEFunction, ::FESpace)` for
      different `::FEFunction` and `::FESpace`. There are two subtasks
      here.
  - Lagrange DOF Basis. [Local Branch](https://github.com/Balaje/Gridap.jl/tree/interpolate_everywhere_arbitrary_points)
    - [x] Making `nlsolve` more robust by providing optional
          parameters. This is for handling somewhat distorted
          elements.
    - [x] Interpolation for Lagrange elements.
    - [x] Finding an effective way to integrate that into
          the package. I thought of modifying
          `FESpaces._cell_vals` to accommodate the new functionality.

  - Raviart Thomas DOF Basis. [Local Branch](https://github.com/Balaje/Gridap.jl/commits/eval_any_monomial_basis_at_single_point)
    - [x] Enable `evaluate` for implementing single point evaluation
          for `AbstractVector{Monomial{D,T}}`. This was done by Eric
          and Alberto. This fixed an error
          that I was getting while evaluating RT Functions at
          arbitrary points.
    - [x] Interpolation for RT Elements. Most of the code is the same
          for Lagrange elements, but I had to invoke
          `ReferenceFEs._eval_moment_dof_basis!` to convert the point
          value to the respective DOF.
    - [x] To find a way to add the interpolation code into the package.


Relevant Code:

> - [Interpolation Lagrange Elements - Sinusoidal
>   Mesh](https://github.com/Balaje/GSoC-2021/blob/main/Interpolation/interpolate.jl)
> - [Interpolation Lagrange Elements - Random
>   Mesh](https://github.com/Balaje/GSoC-2021/blob/main/Interpolation/interpolate_2.jl)
> - [n-Dimensional Lagrange Elements (Test
>   Set)](https://github.com/Balaje/GSoC-2021/blob/main/Interpolation/nDinterpolate.jl)
> - [Interpolation Raviart
>   Thomas
>   Elements](https://github.com/Balaje/GSoC-2021/blob/main/Interpolation/interpolate_rt.jl)
> - [Evaluation of FEFunction on d-1 Manifold (Test
>   Set)](https://github.com/Balaje/GSoC-2021/blob/main/Interpolation/manifold_test.jl)
