---
layout: post
title: Lattice of Subgroups III - Labelling and Coloring Edges
draft_tag: 
- Algebra
- Combinatorics
---

This post will cover the labelling and coloring of edges in the lattice of subgroups of a group. This is useful if one wants to know whether a group has any series of a certain type (e.g. normal, subnormal, central, etc.).

![Lattice of subgroups of $C3:C8$](/images/C3semiC8.png)

<!--more-->

Coloring edges is almost as simple as [coloring vertices]({% post_url 2014-12-17-Subgroup-Lattice-Color-Vertices %}), so we'll start with that. 

## Generating small groups
As we've done in previous posts, let's start by choosing a group and generate its lattice of subgroups. This can be done by referring to this list of [constructions for every group of order less than 32 ](http://www.sagemath.org/doc/constructions/groups.html#construction-instructions-for-every-group-of-order-less-than-32). These instructions allow us to construct every group on Wikipedia's [list of small groups](http://en.wikipedia.org/wiki/List_of_small_groups)! 

For this post, we'll use $G = C_3 \lhd C_8$ (or $\mathbb{Z}_3 \lhd \mathbb{Z}_8$). First, we'll generate $G$ and display it's poset of subgroups. For simplicity, we'll label by cardinality, and we won't color the vertices.

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
# Define group and generate list of subgroups of the group
C3 = CyclicPermutationGroup(3)
alpha = PermutationGroupMorphism(C3,C3,[C3.gen().inverse()])
phi = [[(1,2,3,4,5,6,7,8)],[alpha]]

G = CyclicPermutationGroup(8).semidirect_product(C3,phi)
subgroups = G.subgroups()

# Define f(h,k) = True iff h is a subgroup of k
f = lambda h,k: h.is_subgroup(k)

# Define labels (structure_description requires database_gap package)
label = {subgroups[i] :"." + " "*floor(i/2) + str(len(subgroups[i])) + " "*ceil(i/2) + "." for i in range(len(subgroups))}
# label = {subgroups[i]: "." +" "*floor(i/2) + subgroups[i].structure_description()  + " "*ceil(i/2) + "." for i in range(len(subgroups))}

# Define and display the poset
P = Poset((subgroups, f))
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = 'white')
  </script>
</div>

## Subnormal Series
A [subnormal series](http://en.wikipedia.org/wiki/Subgroup_series#Normal_series.2C_subnormal_series) is a series of subgroups where each subgroup is a normal subgroup of the next group in the series.

In the [previous post]({% post_url 2014-12-17-Subgroup-Lattice-Color-Vertices %}), we colored vertices according to whether the corresponding subgroup was normal (or abelian, or a Sylow subgroup, etc.) These are properties that depend only on each individual subgroup.

However, checking whether a particular series of subgroups is a subnormal series requires checking *pairs* of subgroups to see whether one is a normal subgroup of the other. This suggests that we color *edges* according to whether one of its endpoints is a normal subgroup of the other endpoint.

The edges of the [Hasse diagram](http://en.wikipedia.org/wiki/Hasse_diagram) of a poset are the pairs $(h,k)$ where $h$ is [covered by](http://en.wikipedia.org/wiki/Covering_relation) $k$ in the poset. This means that $h < k$, with nothing else in between. We thus obtain all the edges of a Hasse diagram by calling `P.cover_relations()` on the poset $P$.

To color edges of a graph, we create a dictionary `edge_colors` whose keys are colors, and values are edges that we want to im 
