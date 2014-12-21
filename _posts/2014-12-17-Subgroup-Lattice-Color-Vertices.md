---
layout: post
title: Lattice of Subgroups II - Coloring Vertices
tag: 
- Algebra
- Combinatorics
---

In my [previous post]({% post_url 2014-12-15-Subgroup-Lattice %}), I showed how to use Sage to generate the subgroup lattice of a group, and define labels for the subgroups. In this post, I'll demonstrate how to color subgroups with different colors according to some desired property. If you're not interested in code, scroll to the bottom of the post for a visual collection of groups and their subgroup lattices. 

![Lattice of the dicyclic group $Dic_3$](/images/Dic3Lattice.png "Lattice of the dicyclic group $Dic_3$")

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
label = {subgroups[i] :"." + " "*floor(i/2) + str(len(subgroups[i])) + " "*ceil(i/2) + "." for i in range(len(subgroups))}
# label = {subgroups[i]: "." +" "*floor(i/2) + subgroups[i].structure_description()  + " "*ceil(i/2) + "." for i in range(len(subgroups))}

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
# Frattini subgroup
frattini = G.frattini_subgroup()
# Maximal subgroups
maximals = [x for x in subgroups if P.covers(x,P.top()) and x not in [frattini]]


color = {'lightgreen':[label[x] for x in maximals],
        'white':[label[x] for x in subgroups if x not in maximals + [frattini]],
        'lightblue':[label[x] for x in subgroups if x in [frattini]]
}

P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

## Sylow subgroups
Getting the [Sylow $p$-subgroups](http://mathworld.wolfram.com/Sylowp-Subgroup.html) takes a little more work, since Sage doesn't have a single function that generates all the Sylow subgroups at once.

In Sage, `G.sylow_subgroup(p)` returns *one* Sylow $p$-subgroups. To get *all* the Sylow $p$-subgroups, we could take all conjugates of this Sylow subgroup (since [all Sylow $p$-subgroups are conjugate](http://en.wikipedia.org/wiki/Sylow_theorems#Theorems)). A faster way, however, is to use the fact that the cardinality of all Sylow $p$-subgroups is the maximal $p^{th}$ power dividing the order of $G$.

<div class="linked">
  <script type="text/x-sage">
# Choose some colors we like (can choose more to be safe, in case we have many prime factors)
some_colors = ['lightgreen','pink','yellow','lightblue']

# Get prime factors of |G|
N = G.cardinality()
prime_factors = [p^e for p,e in list(N.factor())]

# List Sylow p-subgroups for each p
sylow = {}
for q in prime_factors:
    sylow[q] = [x for x in subgroups if x.cardinality() == q]
    
# List remaining subgroups
allsylow = sum(sylow.values(),[]) # combine all the sylow subgroups into one list
nonsylow = [x for x in subgroups if x not in allsylow]

# Define colors
color = {'white' : [label[x] for x in nonsylow]}
for c, q in zip(some_colors, prime_factors):
    color[c] = [label[x] for x in subgroups if x in sylow[q]]

# Display the poset
P.plot(element_labels = label, vertex_shape= 'H', vertex_size = 800, vertex_colors = color)
  </script>
</div>

# More examples
<div class="auto">
  <script type="text/x-sage">
# Some small groups
KQ   = [KleinFourGroup(), QuaternionGroup()]
Symm = [SymmetricGroup(N) for N in [1,2,3]]
Alte = [AlternatingGroup(N) for N in [3,4]]
Cycl = [CyclicPermutationGroup(N) for N in [8,12,30,60]]
Dicy = [DiCyclicGroup(N) for N in [3,4,5]]
Dihe = [DihedralGroup(N) for N in [4,5,6,7,8]]

group_list = KQ + Symm + Alte + Cycl + Dicy + Dihe

some_colors = ['lightgreen','pink','yellow','lightblue']

@interact
def subgroup_lattices(Group = selector(values = group_list, buttons=False),
                      Label = selector(values =['None','Generators','Cardinality','Structure Description (requires database_gap)'], default='Cardinality', buttons=False),
                      Color = selector(values =['None','Normal','Abelian and Center','Maximal and Frattini', 'Sylow'], default='Abelian and Center', buttons=False)):
    # Define group and poset of subgroups
    G = Group
    subgroups = G.subgroups()
    P = Poset((subgroups, lambda h,k: h.is_subgroup(k) ))
    
    # Define labels
    label_elements = True
    if Label == 'None':
        label_elements = False
        element_labels = None
    elif Label == 'Generators':        
        element_labels = {x : str(x.gens())[1:-1] for x in subgroups}
    elif Label == 'Cardinality':
        element_labels = {subgroups[i] : "." + " "*floor(i/2) + str(len(subgroups[i])) + " "*ceil(i/2) + "." for i in range(len(subgroups))}
    elif Label == 'Structure Description (requires database_gap)':
        element_labels = {subgroups[i]: "." +" "*floor(i/2) + subgroups[i].structure_description()  + " "*ceil(i/2) + "." for i in range(len(subgroups))}
    
    if label_elements:
        label = element_labels
    else:
        label = {x : x for x in subgroups}
    
    # Define colors
    if Color == 'None':
        color = 'white'
    elif Color == 'Normal':
        color = {'lightgreen':[label[x] for x in subgroups if x.is_normal()],
        'white':[label[x] for x in subgroups if not x.is_normal()]}
    elif Color == 'Abelian and Center':
        color = {'lightgreen':[label[x] for x in subgroups if x != G.center() and x.is_abelian()],
        'white':[label[x] for x in subgroups if not x.is_abelian()],
        'yellow':[label[G.center()]]}
    elif Color == 'Maximal and Frattini':
        frattini = G.frattini_subgroup()
        maximals = [x for x in subgroups if P.covers(x,P.top()) and x not in [frattini]]
        color = {'lightgreen':[label[x] for x in maximals],
        'white':[label[x] for x in subgroups if x not in maximals + [frattini]],
        'lightblue':[label[x] for x in subgroups if x in [frattini]]}
    elif Color == 'Sylow': 
        prime_factors = [p^e for p,e in list(G.cardinality().factor())]
        
        # List Sylow p-subgroups for each p
        sylow = {}
        for q in prime_factors:
            sylow[q] = [x for x in subgroups if x.cardinality() == q]
            
        # List remaining subgroups
        allsylow = sum(sylow.values(),[]) # combine all the sylow subgroups into one list
        nonsylow = [x for x in subgroups if x not in allsylow]
        
        # Define colors
        color = {'white' : [label[x] for x in nonsylow]}
        for c, q in zip(some_colors, prime_factors):
            color[c] = [label[x] for x in subgroups if x in sylow[q]]
        
    # Display poset
    P.plot(label_elements=label_elements, element_labels = element_labels, vertex_shape= 'H', vertex_size = 800, vertex_colors = color).show()
  </script>
</div>

In the next post, we'll look at labelling edges. This is particularly useful if we want to determine if $G$ has  [subgroup series](http://en.wikipedia.org/wiki/Subgroup_series) with certain properties.
