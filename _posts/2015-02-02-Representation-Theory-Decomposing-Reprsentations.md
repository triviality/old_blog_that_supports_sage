---
layout: post
title: Decomposing Representation
draft_tag: 
- Representation Theory
---

In this post, we'll implement an algorithm for decomposing representations that [Dixon published in 1970](http://www.ams.org/journals/mcom/1970-24-111/S0025-5718-1970-0280611-6/S0025-5718-1970-0280611-6.pdf).

<!--more-->

As a motivating example, I'll use the permutation matrix representation of $D_4$ that we saw in an [earlier post]({% post_url 2015-01-20-Representation-Theory-Intro%}){:target="_blank"}. To make the code more generally applicable, let's call the group $G$ and the representation $\rho$:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
G = DihedralGroup(4)

# Defining the permutation representation
def rho(g):
    return g.matrix()

g = G.an_element()
show(rho(g))
  </script>
</div>

We'll see that this is decomposable, and find out what its irreducible components are.

### Unitary representations

A short remark before we begin: The algorithm assumes that $\rho$ is a [unitary representation](http://en.wikipedia.org/wiki/Unitary_representation){:target="_blank"} i.e. for all $g \in G$,

$$ \rho(g)^* \rho(g) = \rho(g) \rho(g)^* = I,$$

where $A*$ is the [conjugate transpose](http://en.wikipedia.org/wiki/Conjugate_transpose){:target="_blank"} of a matrix $A$. For $G$ a finite group, all representations can be made unitary under an appropriate change of basis, so we need not be too concerned about this. In any case, permutation representations are always unitary, so we can proceed with our example.

## Finding non-scalar, commuting matrices

At the end of the [previous post]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"} we saw that in order to decompose a representation $(V,\rho)$, it is enough to find a non-scalar matrix $T$ that commutes with $\rho(g)$ for every $g \in G$.








