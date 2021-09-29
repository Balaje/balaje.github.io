---
layout: post
mathjax: true
tags: [vem, gridap]
title: Virtual element solution with Gridap
excerpt_separator: <!--more-->
---

It is always exciting to explore whether a particular package can be
extended to implement different methods with some of the low-level API
available within the package. So, I wanted to answer the question -
Can `Gridap.jl` can be used to implement the virtual element
method? Virtual element methods can be seen as a generalization of the
finite element method on arbitrary shaped polygon. At the time of
writing this, `Gridap.jl` supports simplices and hypercubes to build
the discretization. However, **it is possible** to use the underlying
machinery to implement the lowest order virtual element method using
`Gridap.jl`. In this post, I will be briefly discussing the
implementation.

<!--more-->

## Introduction

Let us take the following example problem available in
the `README.md` [of the
repository](https://github.com/Balaje/VEM-Julia-Gridap). I won't be
posting the code here, but I strongly recommend that you take a look
at the repository (linked above). But before that, I will talk about
some of the assumption behind this implementation:

1. We use the fact that the degrees of freedom (dofs) of the lowest order VEM
is the same as the lowest order Lagrange dofs, to extend the
`SparseMatrixAssembler` available for the `FESpace` routine to
virtual elements. This can be done using the multiple dispatch
feature available in Julia.
2. We only show this method for building the Ritz-projector
$$\Pi^\nabla$$ for second order problems. Higher order problems,
require higher order projection which is beyond the scope of this
implementation. We will also be dealing with the lowest order
VEM. The $$\Pi^\nabla$$ projection for the lowest order VEM is also
the same as $$\Pi^0$$ projection, which is a bonus here!
3. We will not be implementing any extra quadrature rules here. All
the rules that are necessary are available by default in
`Gridap.jl` via the `∫` method. Higher order polygons require
simplexification which is again beyond the scope of this
implementation.
4. We will also assume that the weak formulation is identical to the
`FEM` formulation. Virtual element method has a different weak
formulation which includes stability terms. This will be managed
differently as we will see in the next section. However, if being
implemented as a package, I presume that VEM weak formulation
requires special treatment.

## Virtual element space and element-wise contributions
To build a lowest order VE space for a discrete model, we use the
new data structure

  ```julia
  P1ConformingVESpace(::DiscreteModel, ::Function; kwargs...):

    model::DiscreteModel
    Π∇
    stability_term
    linear_fespace::FESpace # FESpace(model, ReferenceFE(lagrangian, Float64, 1); kwargs...)
    stab_coeff::Function
  ```
  For the lowest order order case, the virtual element degrees of freedom is identical
  to the lowest order Lagrange finite element space. So for
  geometrical purposes, we can associate a linear `FESpace` to the new
  conforming `VESpace`. In addition to that we have two new data
  structures `Π∇, stability_term` for the purpose of (cellwise) VEM
  projectors. You can find the details of the construction
  [here](http://arturo.imati.cnr.it/brezzi/papers/hitchhikers-preprint.pdf). These
  terms depend entirely on the geometry of the element and can be
  computed while building the space.

Once we construct the `VESpace`, we can construct the stiffness
matrices (local) and the load vector (local) using these functions,
cell-wise:

```julia
  function _generate_mat_contribs(a::Function, V::VESpace, acd, ind)
    area, centroid, diameter = acd[end]
    stab_coeff = get_stab_coeff(V)
    :
    # Some magic here ... #
    :
    coeff_matrix = stab_coeff(centroid)
    projector'*H*projector + (coeff_matrix)*stability_term # Final term
  end
  ```
  Here the `coeff_matrix` is a diagonal matrix of the stability
  function evaluated at the element centroid. This defines the scaling
  of the stability term. In the example, for the weak formulation

  ```julia
  a(u,v) = ∫(K*∇(u)⋅∇(v) + u*v)Qₕ
  ```

the diffusion function `K` is used to define the scaling of the
term, since the weak form scales `~ K`. Also, we multiply the
integrand in the weak form with the cell quadrature `Qₕ =
CellQuadrature(Ω, 4)` rather than the measure `dΩ = Measure(Ω,
4)` as done conventionally in Gridap. This is because we obtain the
cell-wise values as a `LazyArray` rather than a `Dict` and it was
easy to access the cell-wise contribution. This will be done using
`dΩ` in the future.

The load vector similarly can be obtained using the method

  ```julia
  function _generate_vec_contribs(l::Function, V::VESpace, acd, ind)
    a, c, d = acd[ind]
    :
    # More magic ... #
    :
    projector'*Fⱼ
  end
  ```
  where we don't have the stability term. The `magic` here is that
  `Gridap` can be used to compute the action of the bilinear form
  `a(u,v)` on the scaled monomials. This is because the quadrature
  rules on the elements are well defined and direct numerical
  integration is possible (Do take a look at how the implementation
  works in the code
  [here](https://github.com/Balaje/VEM-Julia-Gridap/blob/740985a078551cf92d9815c3e48c319d54e8d150/src/VESpace/AffineVEOperator.jl#L48)
  and
  [here](https://github.com/Balaje/VEM-Julia-Gridap/blob/740985a078551cf92d9815c3e48c319d54e8d150/src/VESpace/AffineVEOperator.jl#L83)). However,
  note that this fails for arbitrary order polygons. The idea is then
  to subdivide the polygons into triangles and dispatch `∫`
  appropriately (Future work).

To obtain the cell-wise contributions, we map the above functions to
each element in the mesh: We do this using the `lazy_map`
functionality.

  ```julia
    matcontribs = lazy_map(i -> _generate_mat_contribs(a, trial, acd, i), 1:num_cells(mesh))
    loadcontribs = lazy_map(i -> _generate_vec_contribs(l, trial, acd, i), 1:num_cells(mesh))
  ```

## Assembly and solution
We then use the low-level assembly routine: `SparseMatrixAssembler`
available in Gridap, but with the extra dispatch

```julia
function SparseMatrixAssembler(trial::VESpace, test::VESpace)
SparseMatrixAssembler(trial.linear_fespace, test.linear_fespace)
end

  # Assemble the matrices
  σₖ = get_cell_dof_ids(trial.linear_fespace)
  matdata = ([matcontribs],[σₖ],[σₖ])
  vecdata = ([loadcontribs], [σₖ])
  assem = SparseMatrixAssembler(trial, test)
  matrix = assemble_matrix(assem, matdata)
  vector = assemble_vector(assem, vecdata)
  ```

We then solve the problem and interpolate the raw vector (VEM
solution) to the underlying `FESpace`, since we cannot express the
virtual element basis explicitly. We perform a very basic
convergence analysis to see if the numbers obey the error estimate

```julia
  || u - uₕ ||₀ ≤ Ch²
  ```
  We run the rate of convergence script `ooc_vem_example.jl` to check:

```julia
julia> err
  5×1 Matrix{Float64}:
    0.040594555904984765
    0.010306757323680199
    0.002586435110433769
    0.0006472156606070523
    0.00016184181015375544

  julia> rate = log.(err[1:end-1] ./ err[2:end])./log.(2)
  4-element Vector{Float64}:
    1.9776957536320143
    1.994553606200162
    1.9986465750207745
    1.9996621557003598
  ```

And thus we are able to verify the rate of convergence result. This
shows that the raw vector we obtain is the virtual element
solution!
