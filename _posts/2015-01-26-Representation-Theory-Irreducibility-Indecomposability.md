---
layout: post
title: Irreducible and Indecomposable Representations
draft_tag: 
- Algebra
---

This post will have more text and less code. Following up from the question at the end of the [previous post]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, I'll define (ir)reducible and (in)decomposable representations, and discuss how we might detect them.

<!--more-->

## Decomposability
Suppose we have a represenation $(V,\rho)$ of a group $G$. If $W$ is a subspace of $V$, then we might ask whether $\rho(g)w \in W$ for all $g \in G, w \in W$. If this is the case, $W$ is a $G$-**invariant** subspace of $V$, and we can form the **subrepresentation** $(W,\rho)$.
