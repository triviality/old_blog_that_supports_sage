---
layout: post
title: Direct Sums and Tensor Products
tag: 
- Algebra
---

In this short post, we will show two ways of combining existing representations to obtain new representations.

<!--more-->

## Recall
In the [previous post]({% post_url 2015-01-20-Representation-Theory-Intro%}){:target="_blank"}, we saw two representations of $D_4$: the permutation representation, and the representation given in this [Wikipedia example](http://en.wikipedia.org/wiki/Dihedral_group#Matrix_representation){:target="_blank"}. Let's first define these in Sage:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
D4 = DihedralGroup(4)

# Defining the permutation representation
def perm(g):
    return g.matrix()

# Defining the representation in the Wikipedia example
r,s = D4.gens()  
D4_dict = {}
for i in range(4):
    for j in range(2):
        D4_dict[r^i * s^j] = (i,j)
        
def wiki_rep(g):
    i,j = D4_dict[g]
    return matrix([[0,-1],[1,0]])^i * matrix([[1,0],[0,-1]])^j

# See what they look like
g = D4.an_element()
show(perm(g))
show(wiki_rep(g))
  </script>
</div>

## Direct Sums

If $(V_1,\rho_1), (V_2,\rho_2)$ are representation of $G$, the [direct sum](http://groupprops.subwiki.org/wiki/Direct_sum_of_linear_representations){:target="_blank"} of these representations is $(V_1 \oplus V_2, \rho)$, where $\rho$ sends $g \in G$ to the [block diagonal matrix](http://en.wikipedia.org/wiki/Block_matrix#Block_diagonal_matrices){:target="_blank"} 

<div class="math">
$$
\begin{pmatrix}
\rho_1(g) & 0 \\
0 & \rho_2(g)
\end{pmatrix}
$$
</div>

Here $\rho_1(g), \rho_2(g)$ and the "zeros" are all *matrices*.

It's best to illustrate with an example. We can define a function `direct_sum` in Sage that takes two representations and returns their direct sum.

<div class="linked">
  <script type="text/x-sage">
def direct_sum(rep1,rep2):
    def new_rep(g):
        return block_diagonal_matrix(rep1(g),rep2(g))
    return new_rep

# Form the direct sum of perm and wiki_rep 
sum_rep = direct_sum(perm,wiki_rep)

# See what it looks like
show(sum_rep(g))
  </script>
</div>

## Tensor products
We can also form the [tensor product](http://groupprops.subwiki.org/wiki/Tensor_product_of_linear_representations){:target="_blank"} $(V_1 \otimes V_2,\rho)$, where $\rho$ sends $g \in G$ to the [Kronecker product](http://en.wikipedia.org/wiki/Kronecker_product){:target="_blank"} of the matrices $\rho_1(g)$ and $\rho_2(g)$.

We define a function `tensor_prod` that takes two representations and returns their tensor product.

<div class="linked">
  <script type="text/x-sage">
def tensor_prod(rep1,rep2):
    def new_rep(g):
        return rep1(g).tensor_product(rep2(g))
    return new_rep

# Form the tensor product of perm and wiki_rep 
prod_rep = tensor_prod(perm,wiki_rep)

# See what it looks like
show(prod_rep(g))  
  </script>
</div>  

Observe that

- $\dim V_1 \oplus V_2 = \dim V_1 + \dim V_2$,
- $\dim V_1 \otimes V_2 = \dim V_1 \times \dim V_2$,

which motivates the terms direct *sum* and tensor *product*.

We can keep taking direct sums and tensor products of existing representations to obtain new ones:

<div class="linked">
  <script type="text/x-sage">
new_rep = direct_sum(perm,tensor_prod(perm,wiki_rep))
show(new_rep(g))  
  </script>
</div>    

## Decomposing representations
Now we know how to build new representations out of old ones. One might be interested in the inverse questions: 

1. Is a given representation a direct sum of smaller representations?
2. Is a given representation a tensor product of smaller representations?

It turns out that Q1 is a much more interesting question to ask than Q2.

A (very poor) analogy of this situation is the problem of "building up" natural numbers. We have two ways of building up new integers from old: we can either add numbers, or multiply them. Given a number $n$, it's easy (and not very interesting) to find smaller numbers that add up to $n$. However, [finding numbers whose product is $n$](http://en.wikipedia.org/wiki/Integer_factorization){:target="_blank"} is *much much* harder (especially for large $n$) and much more [rewarding](http://en.wikipedia.org/wiki/Algebraic_number_theory){:target="_blank"}. Prime numbers also play a special role in the latter case: every positive integer has a unique factorization into primes.

The analogy is a poor one (not least because the roles of "sum" and "product" are switched!). For one, tensor product decompositions are not really as easy as writing a number as a sum of other numbers. However, the analogy motivates the question 

- What are the analogues of "primes" for representations?

We'll try to answer this last question and Q1 in the next few posts, and see what it means for us when working with representations in Sage.
