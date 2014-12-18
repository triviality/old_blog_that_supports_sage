---
layout: post
title: Lattice of Subgroups II : Coloring vertices
tag: Algebra
---

In my [previous post]({% post_url 2014-12-15-Subgroup-Lattice %}), I showed how to use Sage to generate the subgroup lattice of a group, and define labels for the subgroups. In this post, I'll demonstrate how to color subgroups with different colors according to some desired property.

First, let's choose a group we like, and generate its poset. For this post, I'll label the subgroups by their cardinality. If you're trying this code in [SageMathCloud](https://cloud.sagemath.com/) or your own version of  Sage that has the `database_gap` package installed, I strongly recommend labelling the subgroups using `structure_description()` instead).

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
# Define group and generate list of subgroups of the group
G = AlternatingGroup(4)
subgroups = G.subgroups()

# Define f(h,k) = True iff h is a subgroup of k
f = lambda h,k: h.is_subgroup(k)

# Define labels (choose one)
label = {subgroups[i] :"." + " "*i + str(len(subgroups[i])) + " "*i + "." for i in range(len(subgroups))}
# label = {subgroups[i]: "." +" "*(0+i) + subgroups[i].structure_description()  + " "*(0+i) + "." for i in range(len(subgroups))}

# Define and display the poset
P = Poset((subgroups, f))
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800)
  </script>
</div>
