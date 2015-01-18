---
layout: post
title: Subgroup Explorer
tag: 
- Algebra
- Combinatorics
---

![Subgroup Explorer](/images/SubgroupExplorer.png)

I've written an interactive subgroup explorer for all groups of size up to 32. It's powered by Sage and GAP, and allows you to view the subgroup conjugacy classes of a group from your browser.

<!--more-->

Instead of showing the full subgroup lattice, which can get messy for large groups, it only shows the [conjugacy classes of subgroups](http://en.wikipedia.org/wiki/Conjugacy_class#Conjugacy_of_subgroups_and_general_subsets) (i.e. all subgroups that are conjugate are combined into a single vertex).

[Normal subgroups](http://en.wikipedia.org/wiki/Normal_subgroup) are colored green. Additionally, the [center](http://en.wikipedia.org/wiki/Center_%28group_theory%29) is blue while the [commutator subgroup](http://en.wikipedia.org/wiki/Commutator_subgroup) is pink.

The edge labels indicate how many subgroups of one conjugacy class a given representative subgroup of another conjugacy class **contains**, or how many subgroups of one conjugacy class a given representative subgroup of another conjugacy class is **contained by**. The labels are omitted if these numbers are 1. The edge colors indicate whether the subgroups in the "smaller" conjugacy class are normal subgroups of those in "larger" conjugacy class.

In the image above, the group `C3 x (C5 : C4)` (the colon stands for [semi-direct product](http://en.wikipedia.org/wiki/Semidirect_product) and is usually written $\rtimes$) contains 5 subgroups isomorphic to `C12` and 1 subgroup isomorphic to `C3 x D5`. The edge colors indicate that `C3 x D5` is a normal subgroup of `C3 x (C5:C4)` whereas `C12` is not. A table of letters in the group descriptions can be found [here](http://groupprops.subwiki.org/wiki/GAP:StructureDescription#Aspects_of_structure_description).

Click **Go!** below to refresh the viewer, or if it doesn't load.

<div class="go">
  <script type="text/x-sage">
from collections import defaultdict

def subgroup_conj_classes(G):
    """
    Returns [cc_1, cc_2, ... cc_n] : each cc_i is a list containing subgroups of G that belong to the same conjugacy class.
    """
    ccs = G._gap_().ConjugacyClassesSubgroups()
    return [tuple([G.subgroup(gap_group = H) for H in cc.Elements()]) for cc in ccs]

def are_subgroups(cc1,cc2):
    """
    Returns True if some element of cc1 is a subgroup of an element of cc2.
    """    
    # Choose the shorter list to iterate over
    if len(cc1) <= len(cc2):
        h2 = cc2[0]
        for h1 in cc1:
            if h1.is_subgroup(h2):
                return True
    else:
        h1 = cc1[0]
        for h2 in cc2:
            if h1.is_subgroup(h2):
                return True
    return False

group_list = [{}, {}, {'C2': '[(1,2)]'}, {'C3': '[(1,2,3)]'}, {'C2 x C2': '[(1,2), (3,4)]', 'C4': '[(1,2,3,4)]'}, {'C5': '[(1,2,3,4,5)]'}, {'S3': '[(1,2)(3,6)(4,5), (1,5,3)(2,6,4)]', 'C6': '[(1,2)(3,4,5)]'}, {'C7': '[(1,2,3,4,5,6,7)]'}, {'Q8': '[(1,3,4,7)(2,5,6,8), (1,2,4,6)(3,8,7,5)]', 'D4': '[(1,3)(2,5)(4,7)(6,8), (1,2)(3,8)(4,6)(5,7)]', 'C8': '[(1,2,3,4,5,6,7,8)]', 'C2 x C2 x C2': '[(1,2), (3,4), (5,6)]', 'C4 x C2': '[(1,2), (3,4,5,6)]'}, {'C9': '[(1,2,3,4,5,6,7,8,9)]', 'C3 x C3': '[(1,2,3), (4,5,6)]'}, {'D5': '[(1,2)(3,10)(4,9)(5,8)(6,7), (1,3,5,7,9)(2,4,6,8,10)]', 'C10': '[(1,2)(3,4,5,6,7)]'}, {'C11': '[(1,2,3,4,5,6,7,8,9,10,11)]'}, {'D6': '[(1,2)(3,5)(4,10)(6,8)(7,12)(9,11), (1,11,4,3,8,7)(2,12,6,5,10,9)]', 'C12': '[(1,2,3)(4,5,6,7)]', 'C3 : C4': '[(1,2,3,5)(4,10,7,12)(6,11,9,8), (1,8,4)(2,10,6)(3,11,7)(5,12,9)]', 'A4': '[(1,2,5)(3,7,12)(4,11,9)(6,10,8), (1,3)(2,6)(4,8)(5,9)(7,11)(10,12)]', 'C6 x C2': '[(3,4), (1,2)(5,6,7)]'}, {'C13': '[(1,2,3,4,5,6,7,8,9,10,11,12,13)]'}, {'D7': '[(1,2)(3,14)(4,13)(5,12)(6,11)(7,10)(8,9), (1,7,13,5,11,3,9)(2,8,14,6,12,4,10)]', 'C14': '[(1,2)(3,4,5,6,7,8,9)]'}, {'C15': '[(1,2,3)(4,5,6,7,8)]'}, {'C4 x C4': '[(1,2,3,4), (5,6,7,8)]', 'C4 x C2 x C2': '[(1,2), (3,4), (5,6,7,8)]', 'C2 x D4': '[(1,4)(2,7)(3,9)(5,11)(6,12)(8,14)(10,15)(13,16), (1,3)(2,6)(4,9)(5,10)(7,12)(8,13)(11,15)(14,16), (1,2)(3,13)(4,7)(5,8)(6,10)(9,16)(11,14)(12,15)]', 'C2 x Q8': '[(1,4)(2,7)(3,9)(5,11)(6,12)(8,14)(10,15)(13,16), (1,3,5,10)(2,6,8,13)(4,9,11,15)(7,12,14,16), (1,2,5,8)(3,13,10,6)(4,7,11,14)(9,16,15,12)]', 'C8 x C2': '[(1,2), (3,4,5,6,7,8,9,10)]', 'QD16': '[(1,3)(2,6)(4,15)(5,10)(7,16)(8,13)(9,11)(12,14), (1,2,5,8)(3,12,10,16)(4,14,11,7)(6,15,13,9)]', 'C8 : C2': '[(1,3)(2,6)(4,9)(5,10)(7,12)(8,13)(11,15)(14,16), (1,2,4,7,5,8,11,14)(3,13,9,16,10,6,15,12)]', 'Q16': '[(1,3,5,10)(2,6,8,13)(4,15,11,9)(7,16,14,12), (1,2,5,8)(3,12,10,16)(4,14,11,7)(6,15,13,9)]', '(C4 x C2) : C2': '[(1,4,5,11)(2,7,8,14)(3,9,10,15)(6,12,13,16), (1,3)(2,6)(4,9)(5,10)(7,12)(8,13)(11,15)(14,16), (1,2)(3,13)(4,7)(5,8)(6,10)(9,16)(11,14)(12,15)]', 'C2 x C2 x C2 x C2': '[(1,2), (3,4), (5,6), (7,8)]', 'D8': '[(1,3)(2,6)(4,15)(5,10)(7,16)(8,13)(9,11)(12,14), (1,2)(3,12)(4,14)(5,8)(6,9)(7,11)(10,16)(13,15)]', 'C4 : C4': '[(1,3,4,9)(2,6,7,12)(5,10,11,15)(8,13,14,16), (1,2,5,8)(3,12,10,16)(4,7,11,14)(6,15,13,9)]', 'C16': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16)]'}, {'C17': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17)]'}, {'C3 x S3': '[(1,9,4)(2,12,6)(3,14,8)(5,16,11)(7,17,13)(10,18,15), (1,5,7,2,3,10)(4,16,13,12,8,18)(6,14,15,9,11,17)]', 'C6 x C3': '[(6,7,8), (1,2)(3,4,5)]', 'C18': '[(1,2)(3,4,5,6,7,8,9,10,11)]', '(C3 x C3) : C2': '[(1,2)(3,10)(4,12)(5,7)(6,9)(8,18)(11,17)(13,16)(14,15), (1,9,4)(2,12,6)(3,14,8)(5,16,11)(7,17,13)(10,18,15), (1,17,8)(2,18,11)(3,9,13)(4,7,14)(5,12,15)(6,10,16)]', 'D9': '[(1,2)(3,18)(4,12)(5,17)(6,9)(7,16)(8,15)(10,14)(11,13), (1,13,3,9,7,14,4,17,8)(2,15,5,12,10,16,6,18,11)]'}, {'C19': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19)]'}, {'D10': '[(1,2)(3,5)(4,18)(6,16)(7,20)(8,14)(9,19)(10,12)(11,17)(13,15), (1,7,8,15,16,3,4,11,12,19)(2,9,10,17,18,5,6,13,14,20)]', 'C20': '[(1,2,3,4)(5,6,7,8,9)]', 'C10 x C2': '[(3,4), (1,2)(5,6,7,8,9)]', 'C5 : C4': '[(1,2,3,5)(4,10,19,17)(6,11,20,12)(7,13,16,14)(8,18,15,9), (1,4,8,12,16)(2,6,10,14,18)(3,7,11,15,19)(5,9,13,17,20)]'}, {'C7 : C3': '[(1,2,4)(3,8,16)(5,10,12)(6,14,7)(9,20,19)(11,21,15)(13,18,17), (1,18,15,12,9,6,3)(2,20,17,14,11,8,5)(4,21,19,16,13,10,7)]', 'C21': '[(1,2,3)(4,5,6,7,8,9,10)]'}, {'C22': '[(1,2)(3,4,5,6,7,8,9,10,11,12,13)]', 'D11': '[(1,2)(3,22)(4,21)(5,20)(6,19)(7,18)(8,17)(9,16)(10,15)(11,14)(12,13), (1,15,7,21,13,5,19,11,3,17,9)(2,16,8,22,14,6,20,12,4,18,10)]'}, {'C23': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23)]'}, {'C3 x D4': '[(1,2)(3,14)(4,7)(5,8)(6,10)(9,21)(11,15)(12,16)(13,18)(17,24)(19,22)(20,23), (1,9,11,3,4,17)(2,13,15,6,7,20)(5,18,19,10,12,23)(8,21,22,14,16,24)]', 'C24': '[(1,2,3)(4,5,6,7,8,9,10,11)]', 'D12': '[(1,2)(3,13)(4,7)(5,16)(6,9)(8,12)(10,24)(11,22)(14,23)(15,19)(17,21)(18,20), (1,18,11,9,12,10,4,23,5,3,19,17)(2,21,15,13,16,14,7,24,8,6,22,20)]', 'C2 x (C3 : C4)': '[(1,2,4,7)(3,6,9,13)(5,16,11,22)(8,19,15,12)(10,21,17,24)(14,23,20,18), (1,18,5,3,12,10)(2,21,8,6,16,14)(4,23,11,9,19,17)(7,24,15,13,22,20)]', '(C6 x C2) : C2': '[(1,2)(3,13)(4,7)(5,16)(6,9)(8,12)(10,24)(11,22)(14,23)(15,19)(17,21)(18,20), (1,18,5,3,12,10)(2,21,8,6,16,14)(4,23,11,9,19,17)(7,24,15,13,22,20)]', 'S4': '[(1,9,3)(2,13,6)(4,23,11)(5,17,19)(7,24,15)(8,20,22)(10,12,18)(14,16,21), (1,7,12,8)(2,4,16,5)(3,20,19,21)(6,17,22,18)(9,14,23,15)(10,24,11,13)]', 'C4 x S3': '[(1,2)(3,6)(4,7)(5,16)(8,12)(9,13)(10,21)(11,22)(14,18)(15,19)(17,24)(20,23), (1,18,11,9,12,10,4,23,5,3,19,17)(2,21,15,13,16,14,7,24,8,6,22,20)]', 'C6 x C2 x C2': '[(3,4), (5,6), (1,2)(7,8,9)]', 'C2 x C2 x S3': '[(1,3)(2,6)(4,9)(5,10)(7,13)(8,14)(11,17)(12,18)(15,20)(16,21)(19,23)(22,24), (1,2)(3,6)(4,7)(5,16)(8,12)(9,13)(10,21)(11,22)(14,18)(15,19)(17,24)(20,23), (1,19,5,4,12,11)(2,22,8,7,16,15)(3,23,10,9,18,17)(6,24,14,13,21,20)]', 'C2 x A4': '[(1,6,9,2,3,13)(4,15,23,7,11,24)(5,22,17,8,19,20)(10,21,12,14,18,16), (1,5)(2,8)(3,11)(4,12)(6,15)(7,16)(9,18)(10,19)(13,21)(14,22)(17,23)(20,24)]', 'SL(2,3)': '[(1,2,6)(3,8,20)(4,16,13)(5,9,15)(7,14,10)(11,18,24)(12,23,21)(17,22,19), (1,11,5,3)(2,17,9,7)(4,10,12,19)(6,21,15,13)(8,16,18,23)(14,20,22,24)]', 'C12 x C2': '[(6,7,8,9), (1,2)(3,4,5)]', 'C3 : Q8': '[(1,2,4,7)(3,13,9,6)(5,16,11,22)(8,19,15,12)(10,24,17,21)(14,18,20,23), (1,18,11,9,12,10,4,23,5,3,19,17)(2,21,15,13,16,14,7,24,8,6,22,20)]', 'C3 x Q8': '[(1,2,5,8)(3,14,10,6)(4,7,12,16)(9,21,18,13)(11,15,19,22)(17,24,23,20), (1,9,19,10,4,17,5,18,11,3,12,23)(2,13,22,14,7,20,8,21,15,6,16,24)]', 'C3 : C8': '[(1,2,3,6,4,7,9,13)(5,16,10,21,11,22,17,24)(8,18,14,19,15,23,20,12), (1,12,5)(2,16,8)(3,18,10)(4,19,11)(6,21,14)(7,22,15)(9,23,17)(13,24,20)]'}, {'C5 x C5': '[(1,2,3,4,5), (6,7,8,9,10)]', 'C25': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25)]'}, {'C26': '[(1,2)(3,4,5,6,7,8,9,10,11,12,13,14,15)]', 'D13': '[(1,2)(3,26)(4,25)(5,24)(6,23)(7,22)(8,21)(9,20)(10,19)(11,18)(12,17)(13,16)(14,15), (1,19,11,3,21,13,5,23,15,7,25,17,9)(2,20,12,4,22,14,6,24,16,8,26,18,10)]'}, {'C3 x C3 x C3': '[(1,2,3), (4,5,6), (7,8,9)]', 'C9 : C3': '[(1,3,8)(2,6,13)(4,9,16)(5,11,18)(7,14,21)(10,17,23)(12,19,24)(15,22,26)(20,25,27), (1,2,5,4,7,12,10,15,20)(3,14,25,9,22,11,17,6,19)(8,26,24,16,13,27,23,21,18)]', 'C27': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27)]', '(C3 x C3) : C3': '[(1,3,8)(2,6,13)(4,9,16)(5,11,18)(7,14,21)(10,17,23)(12,19,24)(15,22,26)(20,25,27), (1,2,5)(3,14,25)(4,7,12)(6,19,17)(8,26,24)(9,22,11)(10,15,20)(13,27,16)(18,23,21)]', 'C9 x C3': '[(1,2,3), (4,5,6,7,8,9,10,11,12)]'}, {'D14': '[(1,2)(3,5)(4,26)(6,24)(7,28)(8,22)(9,27)(10,20)(11,25)(12,18)(13,23)(14,16)(15,21)(17,19), (1,15,24,11,20,7,16,3,12,27,8,23,4,19)(2,17,26,13,22,9,18,5,14,28,10,25,6,21)]', 'C28': '[(1,2,3,4)(5,6,7,8,9,10,11)]', 'C7 : C4': '[(1,2,3,5)(4,26,7,28)(6,27,9,24)(8,22,11,25)(10,23,13,20)(12,18,15,21)(14,19,17,16), (1,4,8,12,16,20,24)(2,6,10,14,18,22,26)(3,7,11,15,19,23,27)(5,9,13,17,21,25,28)]', 'C14 x C2': '[(3,4), (1,2)(5,6,7,8,9,10,11)]'}, {'C29': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29)]'}, {'C3 x D5': '[(1,5,7,2,3,10)(4,28,13,24,8,30)(6,26,16,21,11,29)(9,23,19,18,14,27)(12,20,22,15,17,25), (1,15,4,21,9)(2,18,6,24,12)(3,20,8,26,14)(5,23,11,28,17)(7,25,13,29,19)(10,27,16,30,22)]', 'C5 x S3': '[(1,9,4)(2,12,6)(3,15,8)(5,18,11)(7,21,14)(10,24,17)(13,26,20)(16,28,23)(19,29,25)(22,30,27), (1,5,7,16,19,2,3,10,13,22)(4,18,14,28,25,12,8,24,20,30)(6,15,17,26,27,9,11,21,23,29)]', 'C30': '[(1,2)(3,4,5)(6,7,8,9,10)]', 'D15': '[(1,2)(3,10)(4,24)(5,7)(6,21)(8,30)(9,18)(11,29)(12,15)(13,28)(14,27)(16,26)(17,25)(19,23)(20,22), (1,25,8,21,19,3,15,13,26,9,7,20,4,29,14)(2,27,11,24,22,5,18,16,28,12,10,23,6,30,17)]'}, {'C31': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31)]'}, {'D16': '[(1,3)(2,7)(4,23)(5,25)(6,13)(8,27)(9,29)(10,19)(11,14)(12,16)(15,31)(17,20)(18,22)(21,32)(24,26)(28,30), (1,2)(3,17)(4,20)(5,22)(6,10)(7,11)(8,14)(9,16)(12,32)(13,28)(15,30)(18,31)(19,24)(21,26)(23,29)(25,27)]', 'C4 x Q8': '[(1,4,16,26)(2,8,22,30)(3,11,25,31)(5,14,6,15)(7,17,29,32)(9,20,10,21)(12,23,13,24)(18,27,19,28), (1,3,5,12)(2,7,9,18)(4,11,14,23)(6,13,16,25)(8,17,20,27)(10,19,22,29)(15,24,26,31)(21,28,30,32), (1,2,6,10)(3,18,13,29)(4,8,15,21)(5,9,16,22)(7,25,19,12)(11,27,24,32)(14,20,26,30)(17,31,28,23)]', '(C4 x C2 x C2) : C2': '[(1,4,5,14)(2,8,9,20)(3,11,12,23)(6,15,16,26)(7,17,18,27)(10,21,22,30)(13,24,25,31)(19,28,29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2)(3,18)(4,21)(5,9)(6,10)(7,12)(8,15)(11,32)(13,29)(14,30)(16,22)(17,31)(19,25)(20,26)(23,28)(24,27)]', 'C4 : Q8': '[(1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3,5,12)(2,7,9,18)(4,11,14,23)(6,13,16,25)(8,17,20,27)(10,19,22,29)(15,24,26,31)(21,28,30,32), (1,2,5,9)(3,18,12,7)(4,21,14,30)(6,10,16,22)(8,26,20,15)(11,32,23,28)(13,29,25,19)(17,24,27,31)]', 'Q8 : C4': '[(1,3,6,13)(2,7,10,19)(4,24,15,11)(5,12,16,25)(8,28,21,17)(9,18,22,29)(14,31,26,23)(20,32,30,27), (1,2,5,9)(3,17,12,27)(4,21,14,30)(6,10,16,22)(7,23,18,11)(8,26,20,15)(13,28,25,32)(19,31,29,24)]', 'C32': '[(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32)]', 'C2 x (C8 : C2)': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2,5,9,6,10,16,22)(3,19,12,29,13,7,25,18)(4,8,14,20,15,21,26,30)(11,28,23,32,24,17,31,27)]', 'C2 x (C4 : C4)': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3,5,12)(2,7,9,18)(4,11,14,23)(6,13,16,25)(8,17,20,27)(10,19,22,29)(15,24,26,31)(21,28,30,32), (1,2,6,10)(3,18,13,29)(4,8,15,21)(5,9,16,22)(7,25,19,12)(11,27,24,32)(14,20,26,30)(17,31,28,23)]', 'C4 x C2 x C2 x C2': '[(1,2), (3,4), (5,6), (7,8,9,10)]', 'C4 . D4 = C4 . (C4 x C2)': '[(1,3,4,11,6,13,15,24)(2,7,8,17,10,19,21,28)(5,12,14,23,16,25,26,31)(9,18,20,27,22,29,30,32), (1,2,5,9,6,10,16,22)(3,17,12,27,13,28,25,32)(4,21,14,30,15,8,26,20)(7,23,18,24,19,31,29,11)]', 'Q32': '[(1,3,6,13)(2,7,10,19)(4,23,15,31)(5,25,16,12)(8,27,21,32)(9,29,22,18)(11,26,24,14)(17,30,28,20), (1,2,6,10)(3,17,13,28)(4,20,15,30)(5,22,16,9)(7,24,19,11)(8,26,21,14)(12,32,25,27)(18,23,29,31)]', '(C4 x C2) : C4': '[(1,3,6,13)(2,7,10,19)(4,11,15,24)(5,12,16,25)(8,17,21,28)(9,18,22,29)(14,23,26,31)(20,27,30,32), (1,2,5,9)(3,17,12,27)(4,8,14,20)(6,10,16,22)(7,23,18,11)(13,28,25,32)(15,21,26,30)(19,31,29,24)]', '(C4 x C4) : C2': '[(1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3,5,12)(2,7,9,18)(4,11,14,23)(6,13,16,25)(8,17,20,27)(10,19,22,29)(15,24,26,31)(21,28,30,32), (1,2)(3,18)(4,21)(5,9)(6,10)(7,12)(8,15)(11,32)(13,29)(14,30)(16,22)(17,31)(19,25)(20,26)(23,28)(24,27)]', 'C4 : C8': '[(1,3,4,11)(2,7,8,17)(5,12,14,23)(6,13,15,24)(9,18,20,27)(10,19,21,28)(16,25,26,31)(22,29,30,32), (1,2,5,9,6,10,16,22)(3,17,12,27,13,28,25,32)(4,8,14,20,15,21,26,30)(7,23,18,24,19,31,29,11)]', 'C2 x QD16': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,11)(5,25)(6,13)(8,17)(9,29)(10,19)(12,16)(14,31)(15,24)(18,22)(20,32)(21,28)(23,26)(27,30), (1,2,6,10)(3,18,13,29)(4,8,15,21)(5,22,16,9)(7,25,19,12)(11,27,24,32)(14,30,26,20)(17,31,28,23)]', '(C2 x D4) : C2': '[(1,5)(2,9)(3,12)(4,14)(6,16)(7,18)(8,20)(10,22)(11,23)(13,25)(15,26)(17,27)(19,29)(21,30)(24,31)(28,32), (1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,24)(5,12)(6,13)(8,28)(9,18)(10,19)(11,15)(14,31)(16,25)(17,21)(20,32)(22,29)(23,26)(27,30), (1,2)(3,19)(4,8)(5,22)(6,10)(7,13)(9,16)(11,28)(12,18)(14,30)(15,21)(17,24)(20,26)(23,27)(25,29)(31,32)]', 'C2 x C2 x D4': '[(1,5)(2,9)(3,12)(4,14)(6,16)(7,18)(8,20)(10,22)(11,23)(13,25)(15,26)(17,27)(19,29)(21,30)(24,31)(28,32), (1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2)(3,19)(4,8)(5,9)(6,10)(7,13)(11,28)(12,29)(14,20)(15,21)(16,22)(17,24)(18,25)(23,32)(26,30)(27,31)]', 'C16 x C2': '[(1,2), (3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18)]', '(C8 x C2) : C2': '[(1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3)(2,7)(4,11)(5,25)(6,13)(8,17)(9,29)(10,19)(12,16)(14,31)(15,24)(18,22)(20,32)(21,28)(23,26)(27,30), (1,2)(3,18)(4,8)(5,22)(6,10)(7,12)(9,16)(11,27)(13,29)(14,30)(15,21)(17,23)(19,25)(20,26)(24,32)(28,31)]', 'C2 x C2 x C2 x C2 x C2': '[(1,2), (3,4), (5,6), (7,8), (9,10)]', 'C2 x ((C4 x C2) : C2)': '[(1,5)(2,9)(3,12)(4,14)(6,16)(7,18)(8,20)(10,22)(11,23)(13,25)(15,26)(17,27)(19,29)(21,30)(24,31)(28,32), (1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2)(3,19)(4,8)(5,9)(6,10)(7,13)(11,28)(12,29)(14,20)(15,21)(16,22)(17,24)(18,25)(23,32)(26,30)(27,31)]', '(C8 : C2) : C2': '[(1,3)(2,7)(4,11)(5,25)(6,13)(8,17)(9,29)(10,19)(12,16)(14,31)(15,24)(18,22)(20,32)(21,28)(23,26)(27,30), (1,2,5,9,6,10,16,22)(3,17,12,27,13,28,25,32)(4,21,14,30,15,8,26,20)(7,31,18,11,19,23,29,24)]', 'C8 x C2 x C2': '[(1,2), (3,4), (5,6,7,8,9,10,11,12)]', 'C2 x C2 x Q8': '[(1,5)(2,9)(3,12)(4,14)(6,16)(7,18)(8,20)(10,22)(11,23)(13,25)(15,26)(17,27)(19,29)(21,30)(24,31)(28,32), (1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3,6,13)(2,7,10,19)(4,11,15,24)(5,12,16,25)(8,17,21,28)(9,18,22,29)(14,23,26,31)(20,27,30,32), (1,2,6,10)(3,19,13,7)(4,8,15,21)(5,9,16,22)(11,28,24,17)(12,29,25,18)(14,20,26,30)(23,32,31,27)]', '(C2 x C2 x C2 x C2) : C2': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2)(3,18)(4,21)(5,9)(6,10)(7,12)(8,15)(11,32)(13,29)(14,30)(16,22)(17,31)(19,25)(20,26)(23,28)(24,27)]', 'C2 . ((C4 x C2) : C2) = (C2 x C2) . (C4 x C2)': '[(1,3,6,13)(2,7,10,19)(4,11,15,24)(5,25,16,12)(8,17,21,28)(9,29,22,18)(14,31,26,23)(20,32,30,27), (1,2,5,9,6,10,16,22)(3,17,12,27,13,28,25,32)(4,21,14,30,15,8,26,20)(7,31,18,11,19,23,29,24)]', 'C4 x C4 x C2': '[(1,2), (3,4,5,6), (7,8,9,10)]', '(C2 x Q8) : C2': '[(1,5)(2,9)(3,12)(4,14)(6,16)(7,18)(8,20)(10,22)(11,23)(13,25)(15,26)(17,27)(19,29)(21,30)(24,31)(28,32), (1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3,6,13)(2,7,10,19)(4,24,15,11)(5,12,16,25)(8,28,21,17)(9,18,22,29)(14,31,26,23)(20,32,30,27), (1,2)(3,19)(4,8)(5,22)(6,10)(7,13)(9,16)(11,28)(12,18)(14,30)(15,21)(17,24)(20,26)(23,27)(25,29)(31,32)]', 'C2 x Q16': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3,6,13)(2,7,10,19)(4,11,15,24)(5,25,16,12)(8,17,21,28)(9,29,22,18)(14,31,26,23)(20,32,30,27), (1,2,6,10)(3,18,13,29)(4,8,15,21)(5,22,16,9)(7,25,19,12)(11,27,24,32)(14,30,26,20)(17,31,28,23)]', 'C8 x C4': '[(1,2,3,4), (5,6,7,8,9,10,11,12)]', '(C2 x C2) . (C2 x C2 x C2)': '[(1,4,5,14)(2,8,9,20)(3,11,12,23)(6,15,16,26)(7,17,18,27)(10,21,22,30)(13,24,25,31)(19,28,29,32), (1,3,6,13)(2,7,10,19)(4,11,15,24)(5,12,16,25)(8,17,21,28)(9,18,22,29)(14,23,26,31)(20,27,30,32), (1,2,5,9)(3,18,12,7)(4,21,14,30)(6,10,16,22)(8,26,20,15)(11,32,23,28)(13,29,25,19)(17,24,27,31)]', 'C4 x D4': '[(1,4,6,15)(2,8,10,21)(3,11,13,24)(5,14,16,26)(7,17,19,28)(9,20,22,30)(12,23,25,31)(18,27,29,32), (1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2,6,10)(3,18,13,29)(4,8,15,21)(5,9,16,22)(7,25,19,12)(11,27,24,32)(14,20,26,30)(17,31,28,23)]', 'QD32': '[(1,3)(2,7)(4,23)(5,25)(6,13)(8,27)(9,29)(10,19)(11,14)(12,16)(15,31)(17,20)(18,22)(21,32)(24,26)(28,30), (1,2,6,10)(3,17,13,28)(4,20,15,30)(5,22,16,9)(7,24,19,11)(8,26,21,14)(12,32,25,27)(18,23,29,31)]', '((C4 x C2) : C2) : C2': '[(1,3)(2,7)(4,11)(5,25)(6,13)(8,17)(9,29)(10,19)(12,16)(14,31)(15,24)(18,22)(20,32)(21,28)(23,26)(27,30), (1,2,5,9)(3,17,12,27)(4,21,14,30)(6,10,16,22)(7,31,18,24)(8,26,20,15)(11,19,23,29)(13,28,25,32)]', 'C8 : C4': '[(1,3,15,24,6,13,4,11)(2,7,21,28,10,19,8,17)(5,12,26,31,16,25,14,23)(9,18,30,32,22,29,20,27), (1,2,5,9)(3,17,12,27)(4,21,14,30)(6,10,16,22)(7,23,18,11)(8,26,20,15)(13,28,25,32)(19,31,29,24)]', 'C2 x D8': '[(1,4)(2,8)(3,11)(5,14)(6,15)(7,17)(9,20)(10,21)(12,23)(13,24)(16,26)(18,27)(19,28)(22,30)(25,31)(29,32), (1,3)(2,7)(4,11)(5,25)(6,13)(8,17)(9,29)(10,19)(12,16)(14,31)(15,24)(18,22)(20,32)(21,28)(23,26)(27,30), (1,2)(3,18)(4,8)(5,22)(6,10)(7,12)(9,16)(11,27)(13,29)(14,30)(15,21)(17,23)(19,25)(20,26)(24,32)(28,31)]', 'C16 : C2': '[(1,3)(2,7)(4,11)(5,12)(6,13)(8,17)(9,18)(10,19)(14,23)(15,24)(16,25)(20,27)(21,28)(22,29)(26,31)(30,32), (1,2,4,8,5,9,14,20,6,10,15,21,16,22,26,30)(3,19,11,28,12,29,23,32,13,7,24,17,25,18,31,27)]'}]

@interact
def subgroup_class_lattices(Cardinality= selector(values = range(2,33),default=6)):
    @interact
    def group_select(Group = selector(values = group_list[Cardinality].keys())):
        # Generate group
        G = PermutationGroup(gap(group_list[Cardinality][Group]))
        
        # Poset of conjugacy classes
        sub_classes = subgroup_conj_classes(G)
        poset = Poset((sub_classes,are_subgroups)) 
        
        # Define vertex labels
        vertex_labels = {sub_classes[0] : '1', sub_classes[-1] : Group}
        if len(sub_classes)>2:
            for cc in sub_classes[1:-1]:
                for desc,gens in group_list[cc[0].cardinality()].items():
                    if cc[0].is_isomorphic(PermutationGroup(gap(gens))):
                        vertex_labels[cc] = desc
                        break        
        
        @interact
        def display_options(Vertex_Colors = selector(values = ['Normal (green), Commutator (pink), Center (blue)','None']), 
                            Edge_Colors = selector(values = ['Is normal subgroup of','None',]), 
                            Edge_Labels = selector(values =['Contains','Contained by', 'Both','None',])):
            # Define vertex colors
            if Vertex_Colors != 'None':
                vertex_colors = defaultdict(list)
                for cc in sub_classes:
                    # Color non-normal subgroups white
                    if not cc[0].is_normal():
                        vertex_colors['white'].append(cc)
                    else:
                        # Color the commutator subgroup pink
                        if cc[0] == G.subgroup(G.commutator().gens()):
                            vertex_colors['pink'].append(cc)
                        # Color the center lightblue
                        elif cc[0] == G.center():
                            vertex_colors['lightblue'].append(cc)
                        # Color all other normal subgroups green
                        else:
                            vertex_colors['lightgreen'].append(cc)
            else:
                vertex_colors = 'white'

            # Define edge colors
            if Edge_Colors != 'None':
                edge_colors = {'#60D6D6':[],'lightgray':[]}
                for cc1,cc2 in poset.cover_relations():
                    h1 = cc1[0]

                    # Color by whether elts of cc1 are normal subgroups of elts of cc2
                    is_normal = False
                    for h2 in cc2:
                        if h1.is_subgroup(h2) and h1.is_normal(h2):
                            edge_colors['#60D6D6'].append((cc1,cc2))
                            is_normal = True
                            break
                    if not is_normal:
                        edge_colors['lightgray'].append((cc1,cc2))
            else:
                edge_colors = None

            #### END OF CUSTOM DISPLAY OPTIONS

            # Define heights, if poset is ranked
            rank_function = poset.rank_function()
            if rank_function:
                heights = defaultdict(list)
                for i in poset:
                    heights[rank_function(i)].append(i)
            else:
                heights = None

            # Generate Hasse diagram
            graph = poset.hasse_diagram()

            # Set edge labels
            label_edges = True
            if Edge_Labels == 'Contained by':
                for cc1,cc2,label in graph.edges():
                    # Count number of subgroups in cc2 that a fixed representative of cc1 is contained by
                    count = sum([cc1[0].is_subgroup(h2) for h2 in cc2])    
                    if count == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + str(count))
            elif Edge_Labels == 'Contains':        
                for cc1,cc2,label in graph.edges():
                    # Count number of subgroups in cc1 that a fixed representative of cc2 contains
                    count = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                    if count == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + str(count))
            elif Edge_Labels == 'Both':    
                for cc1,cc2,label in graph.edges():
                    # Both of the above
                    count1 = sum([cc1[0].is_subgroup(h2) for h2 in cc2])
                    count2 = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                    if count1 == 1 and count2 == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + '{},{}'.format(count1,count2))
            else:
                label_edges = False


            # Generate graph_plot object
            gplot = graph.graphplot(vertex_labels=None,layout='acyclic',vertex_colors = vertex_colors, edge_colors = edge_colors, edge_labels = label_edges, vertex_size = 1000)

            # Set vertex labels
            gplot._plot_components['vertex_labels'] = []
            for v in gplot._nodelist:
                gplot._plot_components['vertex_labels'].append(text(vertex_labels[v],
                    gplot._pos[v], rgbcolor=(0,0,0), zorder=8))

            # Display!
            gplot.show(figsize=(7,7))
  </script>
</div>


Below is the code for a better version that you can run on [SageMathCloud](https://cloud.sagSageMathCloudemath.com). It allows you to input much larger groups. This was used to produce the image at the top of the post. Don't try running it here, however, since the SageCellServer doesn't have the `database_gap` package installed.

<div class="no_eval">
  <script type="text/x-sage">
from collections import defaultdict

def subgroup_conj_classes(G):
    """
    Returns [cc_1, cc_2, ... cc_n] : each cc_i is a list containing subgroups of G that belong to the same conjugacy class.
    """
    ccs = G._gap_().ConjugacyClassesSubgroups()
    return [tuple([G.subgroup(gap_group = H) for H in cc.Elements()]) for cc in ccs]

def are_subgroups(cc1,cc2):
    """
    Returns True if some element of cc1 is a subgroup of an element of cc2.
    """    
    # Choose the shorter list to iterate over
    if len(cc1) <= len(cc2):
        h2 = cc2[0]
        for h1 in cc1:
            if h1.is_subgroup(h2):
                return True
    else:
        h1 = cc1[0]
        for h2 in cc2:
            if h1.is_subgroup(h2):
                return True
    return False

@interact
def subgroup_class_lattices(Cardinality= 6):
    group_list = {Cardinality: {}}
    for G_gap in gap.AllSmallGroups(Cardinality):
        G = PermutationGroup(list(gap.GeneratorsOfGroup(G_gap.AsPermGroup())))
        group_list[Cardinality][G.structure_description()] = str(G.gens())
    @interact
    def group_select(Group = selector(values = group_list[Cardinality].keys())):
        # Generate group
        G = PermutationGroup(gap(group_list[Cardinality][Group]))
        
        # Poset of conjugacy classes
        sub_classes = subgroup_conj_classes(G)
        poset = Poset((sub_classes,are_subgroups)) 
        
        @interact
        def display_options(Vertex_Colors = selector(values = ['Normal (green), Commutator (pink), Center (blue)','None']), 
                            Edge_Colors = selector(values = ['Is normal subgroup of','None',]), 
                            Edge_Labels = selector(values =['Contains','Contained by', 'Both','None',])):
            # Define vertex colors
            if Vertex_Colors != 'None':
                vertex_colors = defaultdict(list)
                for cc in sub_classes:
                    # Color non-normal subgroups white
                    if not cc[0].is_normal():
                        vertex_colors['white'].append(cc)
                    else:
                        # Color the commutator subgroup pink
                        if cc[0] == G.subgroup(G.commutator().gens()):
                            vertex_colors['pink'].append(cc)
                        # Color the center lightblue
                        elif cc[0] == G.center():
                            vertex_colors['lightblue'].append(cc)
                        # Color all other normal subgroups green
                        else:
                            vertex_colors['lightgreen'].append(cc)
            else:
                vertex_colors = 'white'

            # Define edge colors
            if Edge_Colors != 'None':
                edge_colors = {'#60D6D6':[],'lightgray':[]}
                for cc1,cc2 in poset.cover_relations():
                    h1 = cc1[0]

                    # Color by whether elts of cc1 are normal subgroups of elts of cc2
                    is_normal = False
                    for h2 in cc2:
                        if h1.is_subgroup(h2) and h1.is_normal(h2):
                            edge_colors['#60D6D6'].append((cc1,cc2))
                            is_normal = True
                            break
                    if not is_normal:
                        edge_colors['lightgray'].append((cc1,cc2))
            else:
                edge_colors = None

            # Define vertex labels
            vertex_labels = {cc : cc[0].structure_description() for cc in sub_classes}
            #vertex_labels = {cc : cc[0].cardinality() for cc in sub_classes}

            #### END OF CUSTOM DISPLAY OPTIONS

            # Define heights, if poset is ranked
            rank_function = poset.rank_function()
            if rank_function:
                heights = defaultdict(list)
                for i in poset:
                    heights[rank_function(i)].append(i)
            else:
                heights = None

            # Generate Hasse diagram
            graph = poset.hasse_diagram()

            # Set edge labels
            label_edges = True
            if Edge_Labels == 'Contained by':
                for cc1,cc2,label in graph.edges():
                    # Count number of subgroups in cc2 that a fixed representative of cc1 is contained by
                    count = sum([cc1[0].is_subgroup(h2) for h2 in cc2])    
                    if count == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + str(count))
            elif Edge_Labels == 'Contains':        
                for cc1,cc2,label in graph.edges():
                    # Count number of subgroups in cc1 that a fixed representative of cc2 contains
                    count = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                    if count == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + str(count))
            elif Edge_Labels == 'Both':    
                for cc1,cc2,label in graph.edges():
                    # Both of the above
                    count1 = sum([cc1[0].is_subgroup(h2) for h2 in cc2])
                    count2 = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                    if count1 == 1 and count2 == 1:
                        graph.set_edge_label(cc1,cc2,'')
                    else:
                        graph.set_edge_label(cc1,cc2,'  ' + '{},{}'.format(count1,count2))
            else:
                label_edges = False


            # Generate graph_plot object
            gplot = graph.graphplot(vertex_labels=None,layout='acyclic',vertex_colors = vertex_colors, edge_colors = edge_colors, edge_labels = label_edges, vertex_size = 1000)

            # Set vertex labels
            gplot._plot_components['vertex_labels'] = []
            for v in gplot._nodelist:
                gplot._plot_components['vertex_labels'].append(text(vertex_labels[v],
                    gplot._pos[v], rgbcolor=(0,0,0), zorder=8))

            # Display!
            gplot.show(figsize=(7,7))
  </script>
</div>            

Finally, while verifying the results of this program, I found an error in [this book](http://www.cambridge.org/us/academic/subjects/mathematics/algebra/representations-groups-computational-approach)!
The correction has been pencilled in. The original number printed was 1.
![A5 Lattice](/images/A5Lattice_CompareSmall.jpg)