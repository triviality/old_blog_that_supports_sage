---
layout: post
title: Character Theory Basics
draft_tag: 
- Representation Theory
---

This post illustrates some of SageMath's character theory functionality, as well as some basic results about characters of finite groups. 

<!--more-->

## Basic Definitions and Properties

Given a representation $(V,\rho)$ of a group $G$, its [**character**](http://en.wikipedia.org/wiki/Character_theory){:target="_blank"} is a map $ \chi: G \to \mathbb{C}$ that returns the [trace](http://en.wikipedia.org/wiki/Trace_(linear_algebra)){:target="_blank"} of the matrices given by $\rho$:

$$
\chi(g) = \text{trace}(\rho(g)).
$$

A character $\chi$ is **irreducible** if the corresponding $(V,\rho)$ is [irreducible]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"}.

Despite the simplicity of the definition, the (irreducible) characters of a group contain a surprising amount of information about the group. Some [big theorems](http://en.wikipedia.org/wiki/Character_theory#Applications){:target="_blank"} in group theory depend heavily on character theory.

Let's calculate the character of the permutation representation of $D_4$. We'll display pairs $[\rho(g),\chi(g)]$ for each $g \in G$:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
# Define group and its permutation representation
G = DihedralGroup(4)

def rho(g):
    return g.matrix()

# Define a function that returns the character of a representation
def character(rho):
    def chi(g):
        return rho(g).trace()
    return chi

# Compute the character
chi = character(rho)

for g in G:
    show([rho(g),chi(g)])
  </script>
</div>

Many of the following properties of characters can be deduced from properties of the trace:

1. The **dimension** of a character is the dimension of $V$ in $(V,\rho)$. Since $\rho(\text{Id})$ is always the identity matrix, the dimension of $\chi$ is $\chi(\text{Id})$.
1. Because the trace is [invariant under similarity transformations](http://en.wikipedia.org/wiki/Similarity_invariance){:target="_blank"}, $\chi(hgh^{-1}) = \chi(g)$ for all $g,h \in G$. So characters are constant on conjugacy classes, and are thus [**class functions**](http://en.wikipedia.org/wiki/Class_function){:target="_blank"}.
1. Let $\chi_V$ denote the character of $(V,\rho)$. Recalling the definitions of [direct sums and tensor products]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, we see that
  
    -$\chi_{V_1 \oplus V_2} &= \chi_{V_1} + \chi_{V_2}$, 
    -$\chi_{V_1 \otimes V_2} &= \chi_{V_1} \times \chi_{V_2}$ 

## The Character Table

Let's ignore the representation $\rho$ for now, and just look at the character $\chi$:

<div class="linked">
  <script type="text/x-sage">
[chi(g) for g in G]
  </script>
</div>

This is succinct, but we can make it even shorter. From point 2. above, $\chi$ is constant on 

## The orthogonality relations









