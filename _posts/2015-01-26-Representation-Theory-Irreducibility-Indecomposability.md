---
layout: post
title: Irreducible and Indecomposable Representations
tag: 
- Algebra
---

Following up from the questions I asked at the end of the [previous post]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, I'll define (ir)reducible and (in)decomposable representations, and discuss how we might detect them. Unlike previous posts, this post will have just text, and no code. This discussion will form the basis of the algorithm in the next post.

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

Equivalently, $(V,\rho)$ is decomposable if there is an invertible matrix $P$ such that for all $g\in G$,

$$
P^{-1}\rho(g)P = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix},
$$

and indecomposable otherwise. Here, $P$ is a change of basis matrix and conjugating by $P$ changes from the standard basis to the basis given by the columns of $P$. 

## Reducibility

Notice that if $\rho(g)$ were block diagonal, then writing $v \in V$ as ${v_1 \choose v_2}$, where $v_1$ and $v_2$ are vectors whose dimensions agree with the blocks of $\rho(g)$, we see that

$$
\rho(g)v = 
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}{v_1 \choose v_2}
= {\rho_1(g) v_1 \choose \rho_2(g) v_2}.
$$

Let $V_1$ be the subspace of $V$ corresponding to vectors of the form ${v_1 \choose 0}$, and $V_2$ be the subspace of vectors of the form ${0 \choose v_2}$. Then for all $g \in G, v \in V_i$,

$$\rho(g) v = \rho_i(g) v_i \in V_i.$$

Now suppose instead that for all $g \in G, \rho(g)$ has the block upper-triangular form

$$
\rho(g) = 
\begin{pmatrix}
\rho_1(g) & * \\
0 & \rho_2(g)
\end{pmatrix}, 
$$

where $ \* $ represents an arbitrary matrix (possibly different for each $g \in G$). If $\*$ is not the zero matrix for some $g$, then we will still have $\rho(g) v \in V_1 \,\, \forall v \in V_1$, but we no longer have $\rho(g) v \in V_2 \,\, \forall v \in V_2$. In this case, we say that $V_1$ is a subrepresentation of $V$ whereas $V_2$ is not.

Formally, if we have a subspace $W \subset V$ such that for all $g \in G, w \in W$,

$$\rho(g)w \in W,$$

then $W$ is a $G$-**invariant** subspace of $V$, and $(W,\rho)$ is a  **subrepresentation** of $(V,\rho)$.

Any representation $(V,\rho)$ has at least two subrepresentations: $(0,\rho)$ and $(V,\rho)$. If there are no other subrepresentations, then $(V,\rho)$ is [**irreducible**](http://en.wikipedia.org/wiki/Irreducible_representation){:target="_blank"}. Otherwise, it is **reducible**.

Equivalently, $(V,\rho)$ is reducible if there is an invertible matrix $P$ such that for all $g \in G$,

$$
P^{-1}\rho(g)P = 
\begin{pmatrix}
\rho_1(g) & * \\
0 & \rho_2(g)
\end{pmatrix}, 
$$

and irreducible otherwise.

## Maschke's Theorem

Note that a decomposable representation is also reducible, but the converse is not generally true.
(Equivalently: an irreducible representation is also indecomposable, but the converse is not generally true.)
[Maschke's Theorem](http://en.wikipedia.org/wiki/Maschke%27s_theorem){:target="_blank"} tells us that the converse is true over fields of characteristic zero! 

In other words, suppose $V$ is a vector space over a field of characteristic zero, say $\mathbb{C}$, and $(V,\rho)$ has a subrepresentation $(W_1,\rho)$. Then there is a subspace $W_2 \subset V$ such that $(V,\rho)$ is given by the direct sum of $(W_1,\rho)$ and $(W_2,\rho)$. Such a $W_2$ is called a direct complement of $W_1$.

Since we will be working over $\mathbb{C}$, we can thus treat (in)decomposability as equivalent to (ir)reducibility. To understand representations of $G$, we need only understand its irreducible representations, because any other representation can be decomposed into a direct sum of irreducibles.

## Schur's Lemma

How may we detect (ir)reducible representations? We'll make use of the following linear algebraic properties:

Given an eigenvalue $\lambda$ of a matrix $A \in \mathbb{C}^{n \times n}$, its $\lambda$-eigenspace is

$$
E_\lambda = \{v \in \mathbb{C}^n: Av = \lambda v \}.
$$

Clearly, each eigenspace is an invariant subspace of $A$. Now suppose we have another matrix $B \in \mathbb{C}^{n \times n}$ such that $AB = BA$, then $B$ preserves the eigenspaces of $A$ as well. To see this, take $v \in E_\lambda$, then

$$
A(Bv) = B(Av) = B(\lambda v) = \lambda (Bv),
$$

so $Bv \in E_\lambda$, which means that $E_\lambda$ is also an invariant subspace of $B$!

Now suppose we have a representation $(V,\rho)$ and a linear map $T:V \to V$ such that for all $g \in G, v \in V$,

$$
\rho(g)(Tv) = T \rho(g)(v).
$$

Treating $T$ as a matrix, this is equivalent to saying that $\rho(g)T = T\rho(g)$ for all $g \in G$. In that case, the eigenspaces of $T$ are $G$-invariant subspaces, and will yield decompositions of $(V,\rho)$  if they are not the whole of $V$. But if $E_\lambda = V$, then $Tv = \lambda v$ for all $v \in V$, so in fact $T = \lambda I$, where $I$ is the identity matrix. We have thus shown a variant of [Schur's lemma](http://en.wikipedia.org/wiki/Schur%27s_lemma){:target="_blank"}:

*If $(V,\rho)$ is irreducible, and $T$ is such that $\rho(g) T = T \rho(g)$ for all $g \in G$, then $T =\lambda I$ for some $\lambda$. *

We already know that scalar matrices (i.e. matrices of the form $\lambda I$) commute with all matrices. If $(V,\rho)$ is irreducible, then this result says that there are no other matrices that commute with all $\rho(g)$. 

The converse is also true:

*If $(V,\rho)$ is a reducible, then there is some $T \neq \lambda I$ for any $\lambda$ such that $\rho(g) T = T\rho(g)$ for all $g \in G$. *

I won't prove this. Instead, observe that if we can find such a $T$, then its eigenspaces will give a decomposition of $(V,\rho)$. This will be the subject of the next post.









