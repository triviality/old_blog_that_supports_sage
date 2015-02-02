---
layout: post
title: Decomposing Representation
draft_tag: 
- Representation Theory
---

In this post, we'll implement Dixon's 1970 algorithm for decomposing representations.

<!--more-->

Recall the permutation matrix representation of $D_4$ that we saw in an earlier post:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
D4 = DihedralGroup(4)

# Defining the permutation representation
def perm(g):
    return g.matrix()
  </script>
</div>

We will show that this is decomposable, and find out what its irreducible components are.

