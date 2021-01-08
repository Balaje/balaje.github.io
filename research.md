---
layout: default
title: Balaje's Research
mathjax : true
youtubeId: lE5mlxDdmaY
---
# Ice Shelf Vibrations

<p style='text-align: center;'>
<img width="660" height="315" src="/img/ice.png" border="2">
</p>

Consider an ice-shelf which is assumed to be a two dimensional elastic
body. The ice-shelf is fixed at the landward end $$x=L$$ (right) and free to
vibrate at the seaward end $$x=0$$ (left). Open ocean of depth $$H$$
exists for $$x<0$$ and a cavity region filled with water exists
underneath the ice-shelf $$0<x<L$$. The fluid flow is governed by the
potential flow theory and the vibration of the ice-shelf is governed
by linear-elasticity theory.

The objective of the problem is to study the vibrations of the
ice-shelf in response to the waves generated in the open ocean
region. The problem is solved using the finite element method and the
displacement of the ice-shelf is shown in the video below.

{% include youtubePlayer.html id=page.youtubeId %}

See the abstract of the
presentation in [34th International Workshop on Water Waves and
Floating
Bodies](https://carma.newcastle.edu.au/meetings/iwwwfb/accepted/abstract-0117.pdf)

# Gradient Recovery for Virtual Element Methods

Gradient recovery methods are popular numerical techniques to
approximate the gradient of the solution. They have super
convergence property and are used in adaptive refinement. Gradient
recovery techniques based on oblique projection are well studied for
the finite element methods.

A gradient recovery technique based on the oblique projection can be
defined for virtual element methods on Polygonal meshes. In the
virtual element setting, the gradient recovery operator projects
$$\nabla u_h$$ by finding $$g_h^k =
\text{Q}_h\left(\frac{\partial u_h}{\partial x_k}\right) \in V_h$$ for
$$k=1,2$$ such that

$$
\begin{equation}
  \sum_{K}\left(\Pi_K^{0}g_h^k, \Pi_K^{0} \mu_j\right)_K =
  \sum_{K}\left(\frac{\partial (\Pi_K^{0} u_h)}{\partial x_k},
    \Pi_K^{0}\mu_j\right)_K.
\end{equation}
$$

with $$(x_1,x_2) = (x,y)$$ and the functions $$\mu_j \in \mathcal{M}_h
:= \text{span}\{\mu_1,\mu_2,\cdots,\mu_N\}$$ satisfy the
bi-orthogonal relation

$$
\begin{equation}
  \left(\Pi^{0}_K \varphi_i, \Pi^{0}_K \mu_j\right)_K = c_j
  \delta_{ij} \quad \forall K \in \mathcal{T}_h.\label{eq:biorth}
\end{equation}
$$

where the scaling factors $$c_j$$ are obtained using mass lumping. We
can observe higher rates of convergence for the gradient which is
shown in the Figure below.

<p style='text-align: center;'>
<img width="660" height="315" src="/img/grad.png" border="2">
</p>

You can find
the
[full article online](https://journal.austms.org.au/ojs/index.php/ANZIAMJ/article/view/14041/2181) published
in the ANZIAM Journal. Do read my [blog post](./2019/11/02/A-Note-on-Gradient-Recovery.html) on how it can be made better!!
