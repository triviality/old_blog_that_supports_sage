---
layout: post
title: The Argument Principle
tag: Complex Analysis
---

Illustrating the [Argument Principle](http://en.wikipedia.org/wiki/Argument_principle).

<!--more-->

<div class="sage">
  <script type="text/x-sage">
z = var('z')  
@interact
def plot_winding(f=('$f$', z^4  + 7*z^3 + z + 6), 
                 Radius = slider(0,10,default=1), 
                 maxT = slider(0,2*pi,default=2*pi,label="max. Theta")):
    curve = lambda R,t: f(R*exp(I*t))
    t = var('t')
    show(parametric_plot((curve(Radius,t).real(),curve(Radius,t).imag()),(t,0,maxT)))
  </script>
</div>
