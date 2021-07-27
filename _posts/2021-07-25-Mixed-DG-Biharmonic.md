---
layout: post
mathjax: true
tags: [wip, discontinuous, Galerkin, gridap]
title: Mixed DG methods for Biharmonic Equation
---

Let us take the biharmonic problem on $$\Omega$$ along with the
clamped conditions on the boundary $$\partial\Omega$$.

$$
\begin{align}
\Delta^2 u = f \quad &\text{in }\Omega\\
u = \nabla u\cdot \mathbf{n} = 0 \quad &\text{on } \partial\Omega
\end{align}
$$

We now split the biharmonic equation into two equations by introducing
an auxiliary variable $$v = \Delta u$$. We then have a system of
equations

$$
\begin{align}
\begin{cases}
\Delta u &= v \\
\Delta v &= f
\end{cases}\quad &\text{in }\Omega\\
u = \nabla u\cdot \mathbf{n} = 0 \quad &\text{on } \partial\Omega
\end{align}
$$

The system is a little tricky to solve since, we do not have a
boundary condition on $$v$$. It is possible to come up with a
discontinuous Galerkin weak formulation, which is the subject of
discussion by [Gudi,
T., Nataraj, N. & Pani,
A.K](https://link.springer.com/article/10.1007/s10915-008-9200-1) in
their article. I will briefly discuss the construction of their scheme
and how a simple modification leads to an increased rate of
convergence.

## Mixed DG Method

Let us divide the domain $$\Omega$$ into shape regular finite elements (consider
triangles). Let

$$
\mathcal{T}_h = \cup_{i=1}^{N_T} K_i
$$

denote the triangulation of the domain, with $$K_i$$ denoting the
$$i$$-th triangle on the domain. Let us denote by $$\Gamma_\text{I}$$ the set
of all interior edges, and $$\Gamma_\text{D}$$ the set of all boundary
edges. Let $$\Gamma = \Gamma_\text{I} \cup \Gamma_\text{D}$$. For each
edge $$e_k = \partial K_i \cap \partial K_j$$, associate a unit normal
$$n_k$$. Note that the triangulation can contain hanging nodes along
each side of the elements.

Since we are dealing with discontinuous functions, we require the
functions from the broken Sobolev space,

$$
H^s(\Omega, \mathcal{T}_h) = \left\{ v \in L^2(\Omega) \,:\, v
|_{K_i} \in H^s(K_i) \; \forall K_i \in \mathcal{T}_h\right\}
$$

Let $$V = H^2(\Omega, \mathcal{T}_h)$$. The jump and average of a
function $$v$$ across the edge $$e_k = \partial K_i \cap \partial
K_j$$ are defined as

$$
\begin{align}
[v] &= v|_{K_i} - v|_{K_j}\\
\{v\} &= \frac{v|_{K_i} + v|_{K_j}}{2}
\end{align}
$$

There are other intricate assumptions on the triangulation which can
be found in the article. But we have all the necessary ingredients to
define the mixed DG method.

1. Let us take the first equation,

    $$
    \Delta u = v
    $$

    multiply by a function $$w \in V$$ and integrate over the domain

    $$
    \int_\Omega (- \Delta u + v)w\,dx = 0
    $$

    As usual proceeding with integration by parts, we obtain

    $$
    \begin{align}
    \sum_{K\in \mathcal{T}_h}\int_K \nabla u\cdot \nabla w\,dx -
    \sum_{e_k \in \Gamma_\text{I}} \int_{e_k} \left\{\frac{\partial
    u}{\partial n_k}\right\}[w]\,ds + \int_\Omega u\,v\,dx = 0
    \end{align}
    $$

    Here we have used the fact that $$\nabla u \cdot \mathbf{n} = 0$$ on
    $$\Gamma_\text{D}$$. For a sufficiently smooth function $$[u] = 0$$ on
    $$\Gamma$$ as $$u = 0$$ on the boundary. Thus adding the term

    <style>
    .boxed{
        border: 1px solid black ;
    }
    </style>
    <div class="boxed">
    $$
    \begin{align}
    \sum_{K\in \mathcal{T}_h}\int_K \nabla u\cdot \nabla w\,dx -
    \sum_{e_k \in \Gamma_\text{I}} \int_{e_k} \left\{\frac{\partial
    u}{\partial n_k}\right\}[w]\,ds -
    \color{red}{\sum_{e_k \in \Gamma_\text{I}} \int_{e_k} \left\{\frac{\partial
    w}{\partial n_k}\right\}[u]\,ds} + \int_\Omega u\,v\,dx = 0
    \end{align}
    $$
    </div>

    does not affect the equation. If $$u = g_1(x) \ne 0$$, then the
    weakly imposed DBC will be transferred to the right hand side.

2. Similarly with the second equation,

    $$
    \Delta v = f
    $$

    multiply a function $$q \in V$$ and integrate over the domain

    $$
    \int_\Omega (-\Delta v + f)q \,dx = 0
    $$

    Applying integration by parts,

    $$
    \begin{align}
    \sum_{K\in \mathcal{T}_h} \int_K \nabla v\cdot \nabla q\,dx -
    \sum_{e_k \in \Gamma} \int_{e_k} \left\{\frac{\partial
    v}{\partial n_k}\right\}[q]\,ds = -\int_\Omega fq\,dx
    \end{align}
    $$

    For a sufficiently smooth function $$v$$, we have $$[v]=0$$ on
    $$\Gamma_\text{I}$$. Also like before, for a sufficiently smooth
    function $$[u] = 0$$ on $$\Gamma$$, the set of all edges in the
    triangulation. If $$u \ne 0$$ on the boundary $$\Gamma_\text{D}$$,
    the weakly imposed boundary condition appears on the RHS. We can
    add the following terms to the equation.

    <div class="boxed">
    $$
    \begin{align}
    \small
    \sum_{K\in \mathcal{T}_h} \int_K \nabla v\cdot \nabla q\,dx -
    \sum_{e_k \in \Gamma} \int_{e_k} \left\{\frac{\partial
    v}{\partial n_k}\right\}[q]\,ds - \color{red}{\sum_{e_k \in \Gamma_\text{I}} \int_{e_k}
    \left\{\frac{\partial q}{\partial n_k}\right\}[v]\,ds} -
    \color{blue}{\sum_{e_k \in \Gamma_\text{I}} \int_{e_k}
    \alpha \,[u][q]\,ds}   = -\int_\Omega fq\,dx
    \end{align}
    $$
    </div>

To clean things up a bit, let us define the bilinear forms

$$
\begin{align}
B(w,q) &= \sum_{K\in \mathcal{T}_h} \int_K \nabla w\cdot \nabla q\,dx -
\sum_{e_k \in \Gamma} \int_{e_k} \left\{\frac{\partial
w}{\partial n_k}\right\}[q]\,ds - \sum_{e_k \in \Gamma_\text{I}} \int_{e_k}
\left\{\frac{\partial q}{\partial n_k}\right\}[w]\,ds\\
J(w,q) &= \sum_{e_k \in \Gamma_\text{I}} \int_{e_k}
\alpha \,[w][q]\,ds
\end{align}
$$

A mixed weak formulation of the biharmonic problem is to find $$(u,v)
\in V\times V$$

$$
\begin{align}
B(w, u) + (u,v) &= 0,\\
B(v, q) - J(v, q) &= (f,q),
\end{align}
$$

for all $$(w,q) \in V\times V$$. Thus the finite dimensional version
is to find $$(u_h,v_h) \in V_h\times V_h$$

$$
\begin{align}
B(w_h, u_h) + (u_h,v_h) &= 0,\\
B(v_h, q_h) - J(v_h, q_h) &= (f,q_h),
\end{align}
$$

for all $$(w_h,q_h) \in V_h\times V_h$$. The discrete space $$V_h$$ can be
defined to be

$$
V_h = \left\{ v \in H^s(\mathcal{T}_h) \; : \; v|_K
    \in \mathbb{P}_k(K)\;\; \text{for all}\;\; K \in \mathcal{T}_h \right\},
$$

where $$\mathbb{P}_k(K)$$ denotes the space of polynomials of degree
$$\le k$$. I want to talk about the $$h$$-rate of convergence as a function of the
penalty term $$\alpha$$:

$$
\alpha |_{e_k} = \sigma_0 |e_k|^{-i}p^2
$$

where $$i=1$$ or $$i=3$$.  It is well known in literature that for the
case of the Non--symmetric interior penalty Galerkin (NIPG) method for
second order elliptic problems, the choice of the penalty parameter
plays a crucial role on the convergence of the solution. If $$i=1$$,
under normal penalization, the convergence in the $$L^2-$$norm for the
piecewise quadratic case is suboptimal. However, for the case when
$$i=3$$, under super penalization, the convergence is optimal.

For the mixed Discontinuous Galerkin Method considered here, we
observe that the penalization term with $$i=1$$ produces optimal
convergence in all cases, as opposed to $$i=3$$ where optimal
convergence rates are observed only for piecewise cubic elements or higher. The
latter case was well studied by [Gudi et
al.](https://link.springer.com/article/10.1007/s10915-008-9200-1). I
did some tests with **FreeFem** a long time ago, and decided to test
this again with **Gridap.jl**. The results are summarized below.

## Results

For conducting the numerical experiment, I chose the following
parameters.

$$
\begin{align}
\Omega = [0,1]^2, \quad \sigma_0 = \frac{2}{p^2}, \quad i=1 \;(or)\; 3
\end{align}
$$

### Example 1

I assume the exact solution $$u(x,y) = x^2(1-x)^2y^2(1-y)^2$$ and
derive the function $$f(x,y)$$ accordingly. For this choice,
fortunately $$u = \nabla u\cdot \mathbf{n} = 0$$ on the boundary. I
was able to go till fifth order approximation. Crazy! The
code is given below:

>[Gridap code for mixed DG Method](https://gist.github.com/Balaje/6c1b16d919b32ed7597e898aa22887b3)

| $$i=3$$ | $$i=1$$ |
| -- | -- |
| ![pena-strength-3](/img/pena-strength-3.png) | ![pena-strength-3](/img/pena-strength-1.png) |

| Polynomial order | Rate | | | | Polynomial order | Rate |
| 1 | [0.385162629714224, -0.19298411397617532, -0.09157831440244953] | | | | 1 | [2.3182389719693415, 2.6385420060588514, 2.7471415176983913] |
| 2 | [3.488793443784129, 2.812970710594007, 2.4539257908212018] | | | | 2 | [3.1977684002157862, 3.5150997231476526, 3.598743824349367] |
| 3 | [5.1583359163871565, 3.8862008830022368, 3.902386557347976] | | | | 3 | [4.191956257143044, 4.320515951122181, 4.388909586529201] |
| 4 | [5.6954009459952015, 4.642506824234219, 4.767405217310341] | | | | 4 | [5.275165296284339, 5.311122390485823, 5.381825604535792] |
| 5 | [6.339560155054661, 5.878408829655482, 5.943586558721739] | | | | 5 | [6.773942816748595, 7.421917491367188, 5.816251478307224] |

In the left-hand side, I show the results of the scheme discussed by
Gudi et al. for $$i=3$$. In the right-hand side, I show the results of
the scheme with the modified penalty parameter $$i=1$$. In the figures
$$k$$ denotes the polynomial order of approximation.

- One thing that
stands out immediately is the rate of convergence for piece-wise
linear approximation. While setting $$i=3$$ does not yield convergence
for linear elements, $$i=1$$ yields optimal (super?) convergence
here.

- For $$i=3$$ we observe sub-optimal convergence in the $$L^2$$ norm
  for piece-wise quadratic elements, while $$i=1$$ yields optimal
  convergence. Although the magnitude of the error seems to be higher
  in $$i=1$$ case. Interesting.

- Higher order elements seem to yield optimal estimates (atleast close
  to) in both cases here. This is probably expected, as seen from the
  original article by [Gudi et
  al.](https://link.springer.com/article/10.1007/s10915-008-9200-1).

### Example 2

Let us run the tests for one more example, $$u(x,y) = sin(\pi
x)\sin(\pi y)$$. It is one of my favourite examples to test for
convergence in elliptic FEM problems.

| $$i=3$$        | $$i=1$$ |
|----------------|---------|
| ![pena-strength-3](/img/2_pena-strength-3.png)  | ![pena-strength-3](/img/2_pena-strength-1.png)  |

| Polynomial order | Rate | | | | Polynomial order | Rate |
| 1 | [1.4976216098717077, -0.3134869880455214, -0.14745817899196942] | | | | 1 | [1.9061182828433856, 1.8918166517873078, 1.9492174319217117] |
| 2 | [3.1732357610750053, 2.944167398145327, 2.7413869078634567] | | | | 2 | [3.6447844352178858, 3.4869699178184193, 3.4527666618440938] |
| 3 | [4.009692250639503, 3.8761949005578864, 3.9567690045552135] | | | | 3 | [4.374295663329007, 4.407392742622328, 4.429619754458289] |
| 4 | [4.799431041442115, 4.753017784982892, 4.916130229035332] | | | | 4 | [5.394633353260038, 5.360978227021701, 5.424275639399925] |
| 5 | [6.366394820730353, 5.932338705279297, 5.96730924887289] | | | | 5 | [6.51986798965317, 6.6303956328730385, 7.505694550988904] |

Mostly the same, except now for $$k=1$$ i.e., linear polynomials we
get the optimal rate of convergence.

## Conclusions

In Symmetric Interior Penalty Galerkin (SIPG) Method, the choice of penalty
parameter $$\sigma_0$$ has mostly been an arbitrary real number that
is sufficiently large. A definite lower bound to my knowledge has not yet
been established. In this case for mixed DG method, however, an upper
bound for the penalty term $$\alpha|_{e_k} = \sigma_0 |e_k|^{-i} p^2$$
may exist. A high value (proportional to $$|e_k|^{-3}$$) may erode the
method into producing sub-optimal rates. A low value (proportional to
$$|e_k|^{-1}$$), and hence a low value of $$\sigma_0$$ could explain
the good rate of convergence. I am working on deriving the error
estimates to see what is happening (it's hard). Or this may all be due
to some odd special case and cannot be generalized. I will update you
with a new blog entry if I find out anything related to this.

## References

Gudi, T., Nataraj, N. & Pani, A.K. Mixed Discontinuous Galerkin Finite
Element Method for the Biharmonic Equation. J Sci Comput 37, 139–161
(2008). [https://doi.org/10.1007/s10915-008-9200-1]([https://doi.org/10.1007/s10915-008-9200-1)
