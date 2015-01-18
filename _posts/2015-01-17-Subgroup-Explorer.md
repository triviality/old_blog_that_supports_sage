---
layout: post
title: Subgroup Explorer
draft_tag: 
- Algebra
- Combinatorics
---

The previous 3 posts detailed 

<div class="auto">
  <script type="text/x-sage">
# Some small groups
KQ   = [KleinFourGroup(), QuaternionGroup()]
Symm = [SymmetricGroup(N) for N in [1,2,3,4,5]]
Alte = [AlternatingGroup(N) for N in [3,4,5,6]]
Cycl = [CyclicPermutationGroup(N) for N in [8,12,30,60]]
Dicy = [DiCyclicGroup(N) for N in [3,4,5,6,7,8]]
Dihe = [DihedralGroup(N) for N in [4,5,6,7,8]]

group_list = KQ + Symm + Alte + Cycl + Dicy + Dihe

@interact
def subgroup_lattices(Group = selector(values = group_list, buttons=False),
                      Label = selector(values =['None','Generators', 'Cardinality','Structure Description (requires database_gap)'], default='Cardinality', buttons=False)):
    # Define group and list of subgroups
    G = Group
    subgroups = G.subgroups()
    
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
    
    # Define and display poset
    P = Poset((subgroups, lambda h,k: h.is_subgroup(k) ))
    P.plot(label_elements=label_elements, element_labels = element_labels, vertex_shape= 'H', vertex_size = 800, vertex_colors = 'white').show()    
  </script>
</div>
