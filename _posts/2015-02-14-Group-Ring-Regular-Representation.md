---
layout: post
title: The Group Ring and the Regular Representation
tag: 
- Representation Theory
---

In the [previous post]()

In this post, we'll implement an algorithm for decomposing representations that [Dixon published in 1970](http://www.ams.org/journals/mcom/1970-24-111/S0025-5718-1970-0280611-6/S0025-5718-1970-0280611-6.pdf){:target="_blank"}.

<!--more-->

As a motivating example, I'll use the permutation matrix representation of $D_4$ that we saw in an [earlier post]({% post_url 2015-01-20-Representation-Theory-Intro%}){:target="_blank"}. To make the code more generally applicable, let's call the group $G$ and the representation $\rho$:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
G = DihedralGroup(4)

# Defining the permutation representation
def rho(g):
    return g.matrix()

g = G.an_element()
show(rho(g))
  </script>
</div>
