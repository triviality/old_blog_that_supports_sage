---
layout: post
title: The Argument Principle
tag: Complex Analysis
---

This post illustrates the [Argument Principle](http://en.wikipedia.org/wiki/Argument_principle).

<!--more-->

In our example, $f$ is some polynomial, and $C$ is (an arc of) a circle of radius $R$ centered at the origin.

The first plot shows the domain of $f$. We plot the roots of $f$ together with $C$. Let $n_1$ be the number of roots contained within $C$.

The second plot shows the range of $f$. We plot $f(C)$, along with a marker at the origin. Let $n_2$ be the number of times the curve [winds](http://en.wikipedia.org/wiki/Winding_number) around the origin. 

You can verify that $n_1 = n_2$. As you vary the radius $R$, observe how $C$ and $f(C)$ change, and how this affects $n_1$ and $n_2$.

<div id="auto">
  <script type="text/x-sage">
z,t = var('z, t')  
@interact
def plot_winding(f=('$f$', z^4  + 5*z^3 + z + 6), 
                 Radius = slider(0,10,default=1), 
                 maxT = slider(0,2*pi,default=2*pi,label="max. Theta")):
    
    # Find roots of the equation (and convert to numerical approximation)
    roots = [(CDF(r).real(),CDF(r).imag()) for r in f.roots(multiplicities=False)]
    # Circle in domain
    circle = lambda R,t: R*exp(I*t)
    # Image of circle in range
    curve = lambda R,t: f(z=R*exp(I*t))    
    
    # Create plots
    plot_roots = scatter_plot(roots,marker="*")
    plot_circle = parametric_plot((circle(Radius,t).real(),circle(Radius,t).imag()),(t,0,maxT),title="Domain")
    plot_zero = scatter_plot([(0,0)],marker="*")
    plot_image = parametric_plot((curve(Radius,t).real(),curve(Radius,t).imag()),(t,0,maxT),title="Range")
    
    # Show plots
    show(plot_roots + plot_circle)
    show(plot_zero + plot_image)
  </script>
</div>
