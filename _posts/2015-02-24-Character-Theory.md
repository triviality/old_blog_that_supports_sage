---
layout: post
title: Character Theory Basics
draft_tag: 
- Representation Theory
---

This posts illustrates some of SageMath's character theory functionality, as well as some basic results about characters of finite groups.

<!--more-->

## Basic Definitions

Given a representation $(V,\rho)$ of a (finite) group $G$, the [character](http://en.wikipedia.org/wiki/Character_theory){:target="_blank"} of $\rho$ is a map $ \chi: G \to \CC$ that returns the [trace](http://en.wikipedia.org/wiki/Trace_(linear_algebra)){:target="_blank"} of the matrices given by $\rho$ i.e.

$$
\chi(g) = \text{trace}(\rho(g)).
$$



