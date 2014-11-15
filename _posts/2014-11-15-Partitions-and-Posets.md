---
layout: post
title: Partitions and Posets
tag: Combinatorics
---

In SAGE, you can get the set of (unordered) partitions of the $N$ element set $\{1,2,\dots,N\}$ using `SetPartitions`:

<div class="sage">
  <script type="text/x-sage">
N = 3
P = SetPartitions(N)
P
  </script>
</div>

Combine this with the `Poset` class to draw the Hasse diagram of the poset of partitions:

<div class="sage">
  <script type="text/x-sage">
def Partition_Poset(X):  return Poset((SetPartitions(X),lambda q,p: q in p.refinements()))
def p_label(p):
    out = ""
    for block in p:
        for elm in block:
            out += str(elm)
        out += "|"
    return out[:-1]
  </script>
</div>
