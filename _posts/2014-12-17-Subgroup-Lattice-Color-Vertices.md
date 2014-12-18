---
layout: post
title: Lattice of Subgroups II - Coloring vertices
draft_tag: Algebra
---

In my [previous post]({% post_url 2014-12-15-Subgroup-Lattice %}), I showed how to use Sage to generate the subgroup lattice of a group, and define labels for the subgroups. In this post, I'll demonstrate how to color subgroups with different colors according to some desired property.

<!--more-->

First, let's rerun the code from the previous post. We'll choose a group we like and generate its poset. For this post, I'll label the subgroups by their cardinality. If you're trying this code in [SageMathCloud](https://cloud.sagemath.com/) or your own version of  Sage that has the `database_gap` package installed, I strongly recommend labelling the subgroups using `structure_description()` instead).

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
# Define group and generate list of subgroups of the group
G = DiCyclicGroup(3)
subgroups = G.subgroups()

# Define f(h,k) = True iff h is a subgroup of k
f = lambda h,k: h.is_subgroup(k)

# Define labels (structure_description requires database_gap package)
label = {subgroups[i] :"." + " "*i + str(len(subgroups[i])) + " "*i + "." for i in range(len(subgroups))}
# label = {subgroups[i]: "." +" "*(0+i) + subgroups[i].structure_description()  + " "*(0+i) + "." for i in range(len(subgroups))}

# Define and display the poset
P = Poset((subgroups, f))
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800)
  </script>
</div>

## Normal subgroups
Now suppose we wish to know which subgroups are [normal](http://en.wikipedia.org/wiki/Normal_subgroup). We can do so with the following:

<div class="linked">
  <script type="text/x-sage">
# Define a coloring dictionary
color = {'lightgreen':[label[x] for x in subgroups if x.is_normal()],
        'pink':[label[x] for x in subgroups if not x.is_normal()]}

# Display the poset
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

Coloring takes place after labelling, so when we're defining the color dictionary, we have to use the (re)labelled vertices rather than the original vertices. Hence the use of `label[x]` instead of just `x`.

## Abelian subgroups
The rest of this post will just be variations on the same theme. Basically, we just have to create a color dictionary based on some property we want to highlight. Suppose we want [abelian](http://en.wikipedia.org/wiki/Abelian_group) subgroups instead:

<div class="linked">
  <script type="text/x-sage">
# Define a coloring dictionary
color = {'lightgreen':[label[x] for x in subgroups if x.is_abelian()],
        'pink':[label[x] for x in subgroups if not x.is_abelian()]}

# Display the poset
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

## Sylow subgroups
Of course, we can have more than 2 colors. Let's say we want to highlight the [Sylow $p$-subgroups](http://mathworld.wolfram.com/Sylowp-Subgroup.html) of our group (and leave the non-Sylow subgroups white, say). 
In Sage, `G.sylow_subgroup(p)` returns *one* Sylow $p$-subgroup. To get *all* the Sylow $p$-subgroups, we'll take all conjugates of this Sylow subgroup (since [all Sylow $p$-subgroups are conjugate](http://en.wikipedia.org/wiki/Sylow_theorems#Theorems))

We also have to list the primes that divide the order of the group (and list some colors as well).
<div class="linked">
  <script type="text/x-sage">
# Choose some colors we like (can choose more to be safe, in case we have many prime factors)
some_colors = ['lightgreen','pink','yellow','lightblue']

# Get prime factors of |G|
N = G.cardinality()
primes = N.prime_divisors()
print "primes: " + str(primes)

# List Sylow p-subgroups for each p
sylow = {}
for p in primes:
    P = G.sylow_subgroup(p)
    sylow[p] = list(set([G.subgroup(P.conjugate(g)) for g in G]))

# List remaining subgroups
allsylow = sum(sylow.values(),[]) # combine all the sylow subgroups into one list
nonsylow = [x for x in subgroups if x not in allsylow]

# Define colors
color = {'white' : [label[x] for x in nonsylow]}
for c, p in zip(some_colors, primes):
    color[c] = [label[x] for x in subgroups if x in sylow[p]]

# Display the poset
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

## The center

## The Frattini subgroup

## Central series

## Composition series

