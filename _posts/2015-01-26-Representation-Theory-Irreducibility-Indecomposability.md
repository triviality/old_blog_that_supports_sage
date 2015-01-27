---
layout: post
title: Irreducible and Indecomposable Representations
draft_tag: 
- Algebra
---

This post will have more text and less code. Following up from the question at the end of the [previous post]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, I'll define (ir)reducible and (in)decomposable representations, and discuss how we might detect them.

<!--more-->
## Decomposability
In the previous post, I showed how to form the direct sum $(V_1 \oplus V2,\rho)$ of two representations $(V_1,\rho_1)$ and $(V_2,\rho_2)$. The matrices given by $\rho$ looked like this:

$$
\rho(g) = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}, \,\, \forall g \in G
$$

A representation $(V,\rho)$ is **decomposable** if there is an invertible matrix $P$ such that
$$
P^{-1}\rho(g)P = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}, \,\, \forall g \in G.
$$
$P$ is a change of basis matrix, and conjugating by $P$ changes from the standard basis to the basis given by the columns of $P$. We can thus say that $(V,\rho)$ is decomposable if there is a basis of $V$ such that each $\rho(g)$ takes the same block diagonal form in that basis.

If $(V,\rho)$ does not admit such a decomposition, it is **indecomposable**.

## Reducibility
Suppose we have a represenation $(V,\rho)$ of a group $G$. For a subspace $W \subset V$, we might ask whether 

$$\rho(g)w \in W \,, \forall g \in G, w \in W.$$

If this is the case, we say that $W$ is a $G$-**invariant** subspace of $V$, and $(W,\rho)$ is a  **subrepresentation** of $(V,\rho)$.

Any representation $(V,\rho)$ has at least two subrepresentations: $(0,\rho)$ and $(V,\rho)$. If there are no other subrepresentations, then $(V,\rho)$ is [**irreducible**](http://en.wikipedia.org/wiki/Irreducible_representation){:target="_blank"}. Otherwise, it is **reducible**.






