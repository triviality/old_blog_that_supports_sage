---
layout: post
title: Holiday Harmonograph
tag: Misc.
---
*(Guest post from the Annals of Harmonography)*
![#138783](/images/harmonograph.png)

<!--more-->

When it's snowing outside (or maybe not),

And your feet are cold (or maybe hot),

When it's dark as day (or bright as night),

And your heart is heavy (and head is light),

What should you do (what should you say)

To make it all right (to make it okay)?

.

Just pick up a pen (a pencil will do),

Set up a swing (or [three](http://www.karlsims.com/harmonograph/), or two),

And while the world spins (or comes to a still),

In your own little room (or on top of a hill),

Let your pendulum sway (in its time, in its way),

And watch as the pen draws your worries away!

.

.

*(Click inside the colored box to choose a color. Then click outside and watch it update.)*

<div class="auto_out">
  <script type="text/x-sage">
d = 0.05
c = 0.05
p = -0.15
k = 0.05
@interact
def _(u=color_selector(default=(.5,.7,.5), label = 'Color:')):
    x(t) = (sin(t*2*pi) + sin((1-c + u[2]*c*2)*t*2*pi) + p*pi)*exp(-d*t)
    y(t) = (sin((1-c+ u[0]*c*2)*t*2*pi + k*u[1]*pi) + cos((1-c + u[2]*c*2)*t*2*pi) + p*pi)*exp(-d*t)
    
    parametric_plot((x(t),y(t)),(t,0,100),color = u, axes= False, plot_points = 3000).show()
  </script>
</div>

### Related Articles:

- 7 celebrities and their harmonographs
- What your harmonograph says about you
- 10 tips for a happier harmonograph
- Harmonograph secrets... revealed!
