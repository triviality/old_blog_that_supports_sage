---
layout: post
title: Holiday Harmonograph
draft_tag: Misc.
---

*(Guest post from the Annals of Harmonography)*

<div class="auto">
  <script type="text/x-sage">
d = 0.05
c = 0.05
p = -0.15
k = 0.05
@interact
def _(u=color_selector(default=(.5,.7,.5), label = '')):
    x(t) = (sin(t*2*pi) + sin((1-c + u[2]*c*2)*t*2*pi) + p*pi)*exp(-d*t)
    y(t) = (sin((1-c+ u[0]*c*2)*t*2*pi + k*u[1]*pi) + cos((1-c + u[2]*c*2)*t*2*pi) + p*pi)*exp(-d*t)
    
    parametric_plot((x(t),y(t)),(t,0,100),color = u, axes= False, plot_points = 3000).show()
  </script>
</div>



