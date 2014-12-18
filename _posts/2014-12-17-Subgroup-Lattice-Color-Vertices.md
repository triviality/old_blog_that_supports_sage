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
        'white':[label[x] for x in subgroups if not x.is_normal()]}

# Display the poset
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

Coloring takes place after labelling, so when we're defining the color dictionary, we have to use the (re)labelled vertices rather than the original vertices. Hence the use of `label[x]` instead of just `x`.

Here are some more examples. They're all just variations of the above.

## Abelian subgroups and the center
Let's say we want to highlight the [abelian](http://en.wikipedia.org/wiki/Abelian_group) subgroups, with special emphasis on the [center](http://en.wikipedia.org/wiki/Center_%28group_theory%29) of the group. We can do this with a slight modification to the color dictionary:

<div class="linked">
  <script type="text/x-sage">
color = {'lightgreen':[label[x] for x in subgroups if x != G.center() and x.is_abelian()],
        'white':[label[x] for x in subgroups if not x.is_abelian()],
        'yellow':[label[G.center()]]
}

P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

## Maximal subgroups and the Frattini subgroup
The [Frattini subgroup](http://en.wikipedia.org/wiki/Frattini_subgroup) is the intersection of all the [maximal subgroups](http://en.wikipedia.org/wiki/Maximal_subgroup) of $G$. We can see this by highlighting the Frattini and maximal subgroups.

Sage has a built-in function for getting the Frattini subgroup. To get the maximal subgroups, however, we'll have to find the elements [covered](http://en.wikipedia.org/wiki/Covering_relation) by the [greatest element (or top)](http://en.wikipedia.org/wiki/Greatest_element) of the poset $P$.

<div class="linked">
  <script type="text/x-sage">
# Maximal subgroups
maximals = [x for x in subgroups if P.covers(x,P.top())]
# Frattini subgroup
frattini = G.frattini_subgroup()

color = {'lightgreen':[label[x] for x in maximals],
        'white':[label[x] for x in subgroups if x not in maximals + [frattini]],
        'lightblue':[label[frattini]]
}

P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

## Sylow subgroups
Getting the [Sylow $p$-subgroups](http://mathworld.wolfram.com/Sylowp-Subgroup.html) takes a little more work, since Sage doesn't have a single function that generates all the Sylow subgroups at once.

In Sage, `G.sylow_subgroup(p)` returns *one* Sylow $p$-subgroups. To get *all* the Sylow $p$-subgroups, we'll take all conjugates of this Sylow subgroup (since [all Sylow $p$-subgroups are conjugate](http://en.wikipedia.org/wiki/Sylow_theorems#Theorems))

By running over the primes that divide the order of the group, we can generate the Sylow $p$-subgroups for various $p$.

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
    a_sylow_p = G.sylow_subgroup(p)
    sylow[p] = list(set([G.subgroup(a_sylow_p.conjugate(g)) for g in G]))

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

# More examples
*Coming soon!)

In the next post, we'll look at labelling edges as well. This is particularly useful if we want to determine the if $G$ is solvable, nilpotent etc.