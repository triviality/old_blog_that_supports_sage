---
layout: post
title: Character Theory Basics
draft_tag: 
- Representation Theory
---

This posts illustrates some of SageMath's character theory functionality, as well as some basic results about characters of finite groups.

<!--more-->

## Basic Definitions and Properties

Given a representation $(V,\rho)$ of a group $G$, its [**character**](http://en.wikipedia.org/wiki/Character_theory){:target="_blank"} is a map $ \chi: G \to \mathbb{C}$ that returns the [trace](http://en.wikipedia.org/wiki/Trace_(linear_algebra)){:target="_blank"} of the matrices given by $\rho$:

$$
\chi(g) = \text{trace}(\rho(g)).
$$

A character $\chi$ is **irreducible** if the corresponding $(V,\rho)$ is [irreducible]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"}.

That's it! Despite the simplicity of the definition, the (irreducible) characters of a group contain a surprising amount of information about the group. Many results in group theory and representation theory can be proved using only characters, and quite a few big results have only been proved using characters.

Many of the following properties of characters can be deduced from properties of the trace:

- The **dimension** of a character is the dimension of $V$ in $(V,\rho)$. Since $\rho(\text{Id})$ is always the identity matrix, the dimension of $\chi$ is $\chi(\text{Id})$.
- Because the trace is invariant under similarity transformations, $\chi(hgh^{-1}) = \chi(g)$ for all $g,h \in G$. Characters are constant on conjugacy classes, and are thus [**class functions**](http://en.wikipedia.org/wiki/Class_function){:target="_blank"}.
- Letting $\chi_V$ denote the character of $(V,\rho)$, and recalling the definitions of [direct sums and tensor products]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, 

$$
\begin{align*}
\chi_{V_1 \oplus V_2} &= \chi_{V_1} + \chi_{V_2}, \\
\chi_{V_1 \otimes V_2} &= \chi_{V_1} \cdot \chi_{V_2}.
\end{align*}
$$

Let's pull up a representaton of $D_4$ 

## The Character Table










