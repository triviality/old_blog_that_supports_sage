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
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}.
$$

A representation $(V,\rho)$ is **decomposable** if there is a basis of $V$ such that each $\rho(g)$ takes this block diagonal form. If $(V,\rho)$ does not admit such a decomposition, it is **indecomposable**.

Equivalently, $(V,\rho)$ is decomposable if there is an invertible matrix $P$ such that

$$
P^{-1}\rho(g)P = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}, \,\, \forall g \in G,
$$

and indecomposable otherwise. Here, $P$ is a change of basis matrix and conjugating by $P$ changes from the standard basis to the basis given by the columns of $P$. 

## Reducibility

Notice that if $\rho(g)$ were block diagonal, then for $v \in V$

$$
\rho(g)v = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}{v_1 \choose v_2}
= {\rho_1(g) v_1 \choose \rho_2(g) v_2}.
$$

(We have written $v$ as ${v_1 \choose v_2}$, where $v_1$ and $v_2$ are vectors whose dimensions agree with the blocks of $\rho(g)$).

Letting $V_1$ be the subspace of $V$ corresponding to vectors of the form ${v_1 \choose 0}$, we see that $\rho(g) v \in V_1 \,\, \forall g \in G,  v \in V_1$.

Observe that this will still be true if $\rho(g)$ has the block upper-triangular form

$$
\rho(g) = 
\begin{pmatrix}
\rho_1(g) & * \\
0 & \rho_2(g)
\end{pmatrix}, \,\, \forall g \in G
$$

where $*$ stands for matrices that may or may not be $0$ (we might have different matrices for differnt $g \in G$).

More generally, if we have a subspace $W \subset V$ such that

$$\rho(g)w \in W \,\, \forall g \in G, w \in W,$$

we say that $W$ is a $G$-**invariant** subspace of $V$, and $(W,\rho)$ is a  **subrepresentation** of $(V,\rho)$.

Any representation $(V,\rho)$ has at least two subrepresentations: $(0,\rho)$ and $(V,\rho)$. If there are no other subrepresentations, then $(V,\rho)$ is [**irreducible**](http://en.wikipedia.org/wiki/Irreducible_representation){:target="_blank"}. Otherwise, it is **reducible**.

Equivalently, $(V,\rho)$ is reducible if there is an invertible matrix $P$ such that

$$
P^{-1}\rho(g)P = 
\begin{pmatrix}
\rho_1(g) & * \\
0 & \rho_2(g)
\end{pmatrix}, \,\, \forall g \in G,
$$

and irreducible otherwise.





