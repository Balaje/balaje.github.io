---
layout: gsocblog
title: GSoC blog
---

I added this little overview of the tasks just after my midterm
evaluation. I figured my mentors and I needed something to keep track
of in detail. This list is different from the task lists present in the
issue tracker of the fork present in my Github page. This mostly
contains tasks that I have finished, but still requires
refinement. It can be considered as a more detailed version of the
list present in the issue tracker. Not All of the topics listed below
has been merged into the organization's `gridap/master`
repository. The ones that have been merged into the repository will be
marked using a check-mark. The rest is mostly Work in Progress. Expect
this to be updated.

- [x] Implementation for evaluating `FEFunction` on arbitrary points.

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

# GSoC blog

These posts are a part of the Google Summer of Code series. The
full blog is
[here](https://balaje.github.io/blog-page.html)!
