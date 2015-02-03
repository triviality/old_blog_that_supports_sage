---
layout: post
title: The Argument Principle
tag: Uncategorized
---

!["Image of a quarter circle in the first quadrant under the map $f(z) = z^4 + z^3 + 5z^2 + 2z + 4$"](/images/Argument.png "Image of a quarter circle in the first quadrant under the map $f(z) = z^4 + z^3 + 5z^2 + 2z + 4$")

<!--more-->

This post illustrates the [Argument Principle](http://en.wikipedia.org/wiki/Argument_principle).

Let $f$ be a polynomial, and $C$ be (an arc of) a circle of radius $R$ centered at the origin.

The code below generates 2 plots. The first plot shows the domain of $f$. We plot the roots of $f$ together with $C$. Let $n_1$ be the number of roots contained within $C$.

The second plot shows the range of $f$. We plot $f(C)$, along with a marker at the origin. Let $n_2$ be the number of times the curve [winds](http://en.wikipedia.org/wiki/Winding_number) around the origin. 

You can verify that $n_1 = n_2$. As you vary the radius $R$, observe how $C$ and $f(C)$ change, and how this affects $n_1$ and $n_2$.

<div class="auto">
  <script type="text/x-sage">
z,t = var('z, t')  
@interact
def plot_winding(f=('$f$', z^4 + z^3 + 5*z^2 + 2*z + 4), 
                 Radius = slider(0,10,default=2), 
                 maxT = slider(0,2*pi,default=pi/2,label="max. Theta")):
    
    # Find roots of the equation (and convert to numerical approximation)
    roots = [(CDF(r).real(),CDF(r).imag()) for r in f.roots(multiplicities=False)]
    
    R = Radius
    
    # Circle in domain
    arc1(t) = R*exp(I*t)
    # Image of circle
    curve1(t) =  f(z=R*exp(I*t))    
    
    # Line from origin
    line2(t) = R*t
    # Image of line from origin
    curve2(t) = f(z=R*t)
    
    # Line to origin
    line3(t) = R*exp(I*maxT)*(1-t)
    # Image of line to origin
    curve3(t) = f(z=R*exp(I*maxT)*(1-t))
    
    # Create plots in domain
    plot_roots = scatter_plot(roots,marker="*")
    plot_arc1 = parametric_plot((arc1(t).real(),arc1(t).imag()),(t,0,maxT),title="Domain", thickness = 2)
    plot_line2 = parametric_plot((line2(t).real(),line2(t).imag()),(t,0,1),color='greenyellow', thickness = 2)
    plot_line3 = parametric_plot((line3(t).real(),line3(t).imag()),(t,0,1),color='red', thickness = 2)
    
    # Create plots in range
    plot_zero = scatter_plot([(0,0)],marker="*")
    plot_curve1 = parametric_plot((curve1(t).real(),curve1(t).imag()),(t,0,maxT),title="Range", thickness = 2)
    plot_curve2 = parametric_plot((curve2(t).real(),curve2(t).imag()),(t,0,1),color='greenyellow',thickness= 2)
    plot_curve3 = parametric_plot((curve3(t).real(),curve3(t).imag()),(t,0,1),color='red', thickness = 2)
    
    # Show plots
    show(plot_roots + plot_arc1 + plot_line2 + plot_line3)
    show(plot_zero + plot_curve1 + plot_curve2 + plot_curve3)
  </script>
</div>
