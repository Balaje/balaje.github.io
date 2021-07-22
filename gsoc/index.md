---
layout: gsocblog
title: GSoC blog
---

I added this little overview of the tasks just after my midterm
evaluation. I figured my mentors and I needed something to keep track
of progress in detail. This list is different from the task lists present in the
issue tracker in my Github repository. The list below mostly
contains tasks that I have finished, but still requires
refinement. It can be considered as a more detailed version of the
list present in the issue tracker. Not all of the topics listed below
has been merged into the organization's `gridap/master`
repository. The ones that have been merged into the repository will be
marked using a check-mark. The rest is mostly Work in Progress.

- [x] Implementation for evaluating `FEFunction` on arbitrary
      points. This covers both Lagrange and RT elements.

- [ ] Evaluating manifolds on arbitrary points. [Local Branch](https://github.com/Balaje/Gridap.jl/commits/GridTopology_for_BoundaryTriangulation)
  - [ ] Extending `GridTopology` for `BoundaryTriangulation` and
        `RestrictedTriangulation`. This fix is required for computing
        the `GridTopology` from the triangulation of a manifold.

- [ ] Extending `interpolate_everywhere(::FEFunction, ::FESpace)` for
      different `::FEFunction` and `::FESpace`. There are two subtasks
      here.
  - Lagrange DOF Basis. [Local Branch](https://github.com/Balaje/Gridap.jl/tree/interpolate_everywhere_arbitrary_points)
    - [ ] Making `nlsolve` more robust by providing optional
          parameters. This is for handling somewhat distorted
          elements.
    - [ ] Interpolation for Lagrange elements.
    - [ ] Finding an effective way to integrate that into
          the package. I thought of modifying
          `FESpaces._cell_vals` to accommodate the new functionality.

  - Raviart Thomas DOF Basis. [Local Branch](https://github.com/Balaje/Gridap.jl/commits/eval_any_monomial_basis_at_single_point)
    - [x] Enable `evaluate` for implementing single point evaluation
          for `AbstractVector{Monomial{D,T}}`. This was done by Eric
          and Alberto. This fixed an error
          that I was getting while evaluating RT Functions at
          arbitrary points.
    - [ ] Interpolation for RT Elements. Most of the code is the same
          for Lagrange elements, but I had to invoke
          `ReferenceFEs._eval_moment_dof_basis!` to convert the point
          value to the respective DOF.
    - [ ] To find a way to add the interpolation code into the package.


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


# GSoC blog

These posts are a part of the Google Summer of Code series. The
full blog is
[here](https://balaje.github.io/blog-page.html)!
