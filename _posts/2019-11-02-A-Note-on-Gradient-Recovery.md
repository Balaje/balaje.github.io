---
layout: post
mathjax: true
title: A note on Gradient Recovery for the Virtual Element Method
tags: [vem,gradient,recovery]
---
I recently published a paper on Gradient Recovery technique based on
oblique projection for the virtual element method in the ANZIAM
journal as a part of CTAC proceedings [1]. The idea behind the gradient
recovery technique is to project the virtual element gradient onto the
$$L^2$$ space using an oblique projection. You can find the finite
element equivalent, for example, in this paper [3].

We proposed that the bi-orthogonal basis $$\mu_j$$ satisfy the
relation

$$
\begin{equation}
\int_K \Pi^0_K\varphi_i \Pi^0_K \mu_j = c_j\delta_{ij} = A^K.
\end{equation}
$$

and then the gradient recovery operator as
$$\text{Q}_h\left(\frac{\partial u_h}{\partial x_k}\right) \in V_h$$ for
$$k=1,2$$ such that

$$
\begin{equation}
\sum_{K}\left(\Pi_K^{0}g_h^k, \Pi_K^{0} \mu_j\right)_K =
\sum_{K}\left(\frac{\partial (\Pi_K^{0} u_h)}{\partial x_k},
\Pi_K^{0}\mu_j\right)_K.
\end{equation}
$$

In the paper, we have shown how this projection can be used to compute
the right hand side of the above equation explicitly without computing
the bi-orthogonal bases. The method worked beautifully yielding a
super-convergence rate of $$\approx 1.5$$ on quasi-uniform
meshes. However, there is a slight trouble in the definition of the
bi-orthogonal bases. We see that, using the ansatz

$$
\begin{equation}
\Pi^0_K \varphi_i = \sum_{\alpha=1}^{n_k} s_i^\alpha m_\alpha,
\end{equation}
$$

and the fact that $$m_\alpha \in V_h$$, we have

$$
\begin{eqnarray}
\int_K \Pi^0_K\varphi_i \Pi^0_K \mu_j = \sum_{\alpha=1}^{n_k}
s_i^\alpha \sum_{r=1}^N \text{dof}_r(m_\alpha) \int_K
\Pi^0_K\varphi_i \Pi^0_K \mu_j,
\end{eqnarray}
$$

which in matrix form yields

$$
A^K = M\Pi^0_* \,A^K,
$$

where $$M = \text{dof}_r(m_\alpha)$$. This does not always hold true! So
now we must redefine the bi-orthogonal bases and the projection
operator. The redefinition *does not* yield a huge difference in
computation except the formation of a new stability type term which is
crucial to ensure good mathematical properties.

## Redefinition of the gradient recovery

Let us redefine the bi-orthogonal bases $$\mu_j$$ as a set of
functions satisfying

$$
\int_K \varphi_i \,\mu_j = c_j\delta_{ij} = D^K,
$$

and the gradient recovery operator as $$\text{Q}_h\left(\frac{\partial
u_h}{\partial x_k}\right) \in V_h$$ for $$k=1,2$$ such that

$$
\begin{equation}
\sum_{K}\left(g_h^k, \mu_j\right)_K =
\sum_{K}\left(\frac{\partial u_h}{\partial x_k},
\mu_j\right)_K.
\end{equation}
$$

We note that this definition projects the gradient itself rather than
the $$L^2-$$ projection and is more rigorous than the definition
presented in the paper. Now we describe how to compute this gradient
recovery.

### The linear system

The left hand side in the gradient recovery equation can be written as

$$
\begin{equation}
\sum_{K}\left(g_h^k, \mu_j\right)_K = \sum_K \sum_{i=1}^{N}
\text{dof}(g_h^k)\, (\varphi_i, \mu_j)_K.
\end{equation}
$$

The right side can be split into two terms as follows.

$$
\begin{eqnarray}
\sum_{K}\left(\frac{\partial u_h}{\partial x_k},
\mu_j\right)_K &=&
\sum_{K}\sum_{i=1}^N\text{dof}_i(u_h)\left(\frac{\partial
\varphi_i}{\partial x_k}, \mu_j\right)_K,\\
&=&
\sum_{K}\sum_{i=1}^N\text{dof}_i(u_h)\left(\frac{\partial
(\Pi^0_K \varphi_i)}{\partial x_k}, \mu_j\right)_K +
\sum_{K}\sum_{i=1}^N\text{dof}_i(u_h)\left(\frac{\partial
(\text{I}-\Pi^0_K)\varphi_i}{\partial x_k}, \mu_j\right)_K.
\end{eqnarray}
$$

which can be written as a matrix system

$$
[D]\left\{g_h^k\right\} = [Q_k]\left\{u_h\right\} + [S_k]\left\{u_h\right\},
$$

where the vector $$\left\{u_h\right\} = \text{dof}_i(u_h)$$ is known.

---
The first term can be computed exactly using the definition of the
bi-orthogonal bases as

$$
\begin{eqnarray}
\left(\frac{\partial (\Pi^0_K\varphi_i)}{\partial x_k}, \mu_j\right)_K
&=& \sum_{\alpha=1}^{n_k} s_i^\alpha \left(\frac{\partial
m_\alpha}{\partial x_k},\mu_j\right)_K,\\
&=& \sum_{\alpha=1}^{n_k} s_i^\alpha \sum_{j=1}^N
\text{dof}_j\left(\frac{\partial m_\alpha}{\partial x_k}\right)
\left(\varphi_i,\mu_j\right)_K.\\
\end{eqnarray}
$$

Thus the global matrix can be written as $$Q_k = \sum_K Q_k^K$$ with
the element level matrix given by

$$
Q_k^K = D^K M_k \Pi^0_*,
$$

where the entries of $$M_k = \text{dof}\left(\frac{\partial m_\alpha}{\partial
x_k}\right)$$ and $$\Pi^0_* = s_i^\alpha$$.

---

The second term however, cannot be computed exactly and must be
approximated. This acts as a stability term for the gradient recovery
operator. We have

$$
\left(\frac{\partial
(\text{I}-\Pi^0_K)\varphi_i}{\partial x_k}, \mu_j\right)_K \approx
\frac{|K|}{h_K} \sum_{k=1}^{N}
\text{dof}_k\left\{(\text{I}-\Pi^0_K)\varphi_i\right\}
\, \text{dof}_k(\mu_j).
$$

We are still not complete. The term $$\text{dof}_k(\mu_j)$$ can be
evaluated since the function is not known on the
element/boundaries. However, we can see that using the definition of
the bi-orthogonal bases, we can write

$$
\begin{eqnarray}
D^K &=& \int_K \left(\Pi^0_K\varphi_i +
(\text{I}-\Pi^0_K)\varphi_i\right)\, \mu_j,\\
&=& \int_K \Pi^0_K\varphi_i\,\mu_j +
\int_K(\text{I}-\Pi^0_K\varphi_i)\, \mu_j.\\
\end{eqnarray}
$$

For the above equation, using similar arguments for the first term, we
can construct the matrix equation

$$
\begin{eqnarray*}
M\Pi^0_*D^K + T_K  &=& D^K\quad \text{or}\\
T_K &=& (\text{I}-M\Pi^0_*) D_K.
\end{eqnarray*}
$$

where $$M = \text{dof}(m_\alpha)$$. Thus, we have

$$
\begin{eqnarray*}
\sum_{k=1}^{N}
\text{dof}_k\left\{(\text{I}-\Pi^0_K)\varphi_i\right\}
\, \text{dof}_k(\mu_j) \approx \frac{1}{|K|}T_K
\end{eqnarray*}
$$

and therefore

$$
S_k^K = \left(\frac{\partial
(\text{I}-\Pi^0_K)\varphi_i}{\partial x_k}, \mu_j\right)_K \approx
\frac{1}{h_K}T_K
$$

and $$S_k = \sum_K S_k^K$$.

---

Thus the redefined gradient recovery can be performed by solving the
linear system

$$
\begin{eqnarray}
[D]\left\{g_h^k\right\} &=& [QS]\left\{u_h\right\},\\
\left\{g_h^k\right\} &=& [D]^{-1}[QS]\left\{u_h\right\}.
\end{eqnarray}
$$

where

$$
QS = \sum_K \left(D^K\,M_k\Pi^0_* + \frac{1}{h_K}T_K\right)
$$

and the sum denotes a finite element assembly over the elements
$$K$$. The diagonal matrix $$D$$ can be easily inverted thus retaining
the computational efficiency.


## Summary

Now the question is - What is the difference between the
redefined method and the earlier method? The answers are:

1. The redefined method is a natural extension of the finite element
gradient recovery using oblique projection.

2. The redefined method has an extra stability term that is absent in
the method proposed in the paper. This is crucial for
mathematically good properties to be ensured. The older method can be
recovered by simply ignoring the stability term in the redefined
method.

The effect of the stability term must be studied in detail and I hope
to write a new post on it soon!

## References

[1] B. Kalyanaraman, B. P. Lamichhane, M. Meylan. "A gradient recovery
method based on an oblique projection for virtual element method". In:
ANZIAM Journal. 60 (2018), pp. 187-200.

[2] L. Beirão da Veiga, F. Brezzi, L. D. Marini, and A. Russo. "The
hitchhiker’s guide to the virtual element method". In:
Math. Mod. Meth. Appl. Sci. 24.08 (2014), pp. 1541–1573.

[3] B. P. Lamichhane. "A gradient recovery operator based on an oblique
projection". In: Electron. Trans. Numer. Anal. 37 (2010),
pp. 166–172.
