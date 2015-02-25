---
layout: post
title: Character Theory Basics
draft_tag: 
- Representation Theory
---

This posts illustrates some of SageMath's character theory functionality, as well as some basic results about characters of finite groups.

<!--more-->

## Basic Definitions and Properties

Given a representation $\rho$ of a group $G$, the [**character**](http://en.wikipedia.org/wiki/Character_theory){:target="_blank"} of $\rho$ is a map $ \chi: G \to \mathbb{C}$ that returns the [trace](http://en.wikipedia.org/wiki/Trace_(linear_algebra)){:target="_blank"} of the matrices given by $\rho$:

$$
\chi(g) = \text{trace}(\rho(g)).
$$

A character $\chi$ is **irreducible** if the corresponding $\rho$ is [irreducible]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"}.

That's it! Despite the simplicity of the definition, the (irreducible) characters of a group contain a surprisingly amount of information about the group. Many results in group theory and representation theory can be proved using only characters, and quite a few big results have only been proved using characters.

The **dimension** of a character is the dimension of $V$ in $(V,\rho)$. Since $\rho(\Id)$ is always the identity matrix, $\dim \chi = \chi(\Id)$.







