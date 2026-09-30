---
layout: post
mathjax: true
tags: [gridap, gsoc]
title: Wrapping up GSoC
---

This is the final post in the Google Summer of Code 2021 series, and
here I wish to summarize the project. **This post is my official
submission of my work for the final evaluation**. During the summer of code, I
wrote five blog posts related to the project which can be found on the
[my GSoC webpage](/gsoc/index.html). Along with the posts, I have also
posted the progress report which can be found at the end of
the page. I will briefly go through the
new features and list all the Pull Requests submitted to the
organization's main repository that contains my contribution. For
demonstrating the new features added, I use the following script:

> [Sample code snippets in my GSoC repo](https://github.com/Balaje/GSoC-2021/blob/main/blog-script.jl)

The script is also available [as a
GitHub gist](https://gist.github.com/Balaje/fa9769bff8fc29945e1220aa29e93afa). Clone
and run the script:

``` julia
shell> git clone https://gist.github.com/Balaje/fa9769bff8fc29945e1220aa29e93afa blog-script
shell> cd blog-script/

julia> include("blog-script.jl")
```

## New features and merged PRs

- Evaluate `FEFunction` at arbitrary points (since Gridap release [v.0.16.0](https://github.com/gridap/Gridap.jl/releases/tag/v0.16.0)):
  - [Allow evaluating FE functions at arbitrary points #523 (Contributed
  via Erik's branch)](https://github.com/gridap/Gridap.jl/pull/523#issuecomment-851219490)
      - [Evaluate CellField on arbitrary points #1 (Erik's branch)](https://github.com/eschnett/Gridap.jl/pull/1)
      - [More tests for evaluate #2 (Erik's
    branch)](https://github.com/eschnett/Gridap.jl/pull/2)

- Fix evaluation for RT Elements (since Gridap release [v.0.16.4](https://github.com/gridap/Gridap.jl/releases/tag/v0.16.4)):
  - [Evaluate any monomial basis at single point
  #628](https://github.com/gridap/Gridap.jl/pull/628)

- Interpolation between `FESpace` (since Gridap release [v.0.16.4](https://github.com/gridap/Gridap.jl/releases/tag/v0.16.4)):
  - [Interpolate everywhere arbitrary points #632](https://github.com/gridap/Gridap.jl/pull/632)

## Evaluating `CellField` on arbitrary point (Complete)

This is the first step in implementing the interpolation
algorithm. After running the script, we type the following in the
Julia REPL:

``` julia
fhs = get_data(fₕ)
evaluate(fhs[1], Point(rand(2)))
```

In the first line, we extract the data using `get_data` from the
`FEFunction` cell-wise,
resulting in a `LazyArray` of `LinearCombinationField`. We can
evaluate a `LinearCombinationField` at any point in Gridap. Now since
finite element functions are defined locally, we need to *search* for
the corresponding cell where the arbitrary point lies. Evaluating the
"local" `FEFunction` at the cell gives the value of the
`FEFunction`. The searching is done by building a `KDTree` from the
set of points on the mesh and then finding the nearest neighbour. This
searching mechanism has an $$O(\log(N))$$ complexity where $$N$$ being
the number of points on the mesh. This is discussed in detail in my
blog post:

> [Google Summer of Code 2021 - Community Bonding](/2021/06/05/GSoC-Week-0.html)

Once we have the required finite element function `fₕ` defined on an `FESpace`,

``` julia
domain = (0,1,0,1)
partition = (5,5)
model = CartesianDiscreteModel(domain, partition)
f₁(x) = x[1] + x[2]
reffe₁ = ReferenceFE(lagrangian, Float64, 1) # Lagrangian ReferenceFE
V₁ = FESpace(model, reffe₁) # Build the FESpace
fₕ = interpolate_everywhere(f₁,V₁)
```

evaluating `fₕ` on an arbitrary point is as simple as

``` julia
fₕ(Point(rand(2))
```

The snippets for Lagrange FESpaces can be found
[here](https://github.com/Balaje/GSoC-2021/blob/a30ad38590a402e0d05cfe26186ad0f73944caf7/blog-script.jl#L23)
and the same can be found for RT elements
[here](https://github.com/Balaje/GSoC-2021/blob/a30ad38590a402e0d05cfe26186ad0f73944caf7/blog-script.jl#L40).

## Interpolation (Complete)

Now interpolation can be done using the new structure `Interpolable`
available in Gridap. This takes advantage of the earlier mechanism of
evaluating functions on arbitrary points. Once we define the
`Interpolable` object corresponding to an `FEFunction`, we can
dispatch the `Interpolable` object to the existing
`interpolate_everywhere` method and interpolate the
function onto the new space `W₁` from `V₁`.

``` julia
domain = (0,1,0,1)
partition = (20,20)
model = CartesianDiscreteModel(domain,partition)
W₁ = FESpace(model, reffe₁)

ifₕ = Interpolable(fₕ)
gₕ = interpolate_everywhere(ifₕ,W₁) # Interpolate fₕ ∈ V₁ on to W₁
```

We had a lot of back and forth on the interface and some of the
possible implementations are available on the blog posts:

>- [Interpolation for Lagrangian Elements - I](/2021/07/05/GSoC-Week-1.html)
>- [Interpolation for Lagrangian Elements - II](/2021/07/05/GSoC-Week-2.html)
>- [Interpolation for Raviart Thomas Basis Functions](/2021/08/01/GSoC-Week-3.html)


But we finally settled on the current interface using `Interpolable`
after a long discussion in
[PR#632](https://github.com/gridap/Gridap.jl/pull/632). Future work
from here include optimizing the interface and reducing memory
allocations even further. Sample snippets from `blog-script.jl` for demonstrating
interpolation can be found for Lagrange elements
[here](https://github.com/Balaje/GSoC-2021/blob/a30ad38590a402e0d05cfe26186ad0f73944caf7/blog-script.jl#L57)
and
Raviart Thomas Element [here](https://github.com/Balaje/GSoC-2021/blob/a30ad38590a402e0d05cfe26186ad0f73944caf7/blog-script.jl#L74).

## Interpolation Matrix (Work In Progress)

Code to generate interpolation matrix. Sketched a way to do it for
`LagrangianDofBasis` and a preliminary version is available on my [GSoC
Repo](https://github.com/Balaje/GSoC-2021/blob/478b3f79bca501639ce5b61d42734655dd301780/Interpolation/imatrix.jl).


## Miscellaneous Examples

Example for testing the interpolation algorithm for
n-Dimensional problems are available on my GSoC repo. I test the
interface for a sinusoidal/random perturbation of the original mesh
[(code)](https://github.com/Balaje/GSoC-2021/blob/478b3f79bca501639ce5b61d42734655dd301780/Interpolation/nDinterpolate.jl). The
repository also contains examples of older implementations discussed
in the earlier blog posts.

## Acknowledgements
I feel great to have worked on this project with Gridap this year!
Many thanks to NumFOCUS, my mentors Eric, Santi, Oriol and the Gridap
team. Looking forward to keep contributing to open-source and Gridap
in the future.
