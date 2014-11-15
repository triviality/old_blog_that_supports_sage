---
layout: post
title: Hullo
---

This blog is powered by the [Sage Cell Server](http://sagecell.sagemath.org/). You can type Sage/Python code into the cell below, and press `Shift+Enter` to evaluate it (or click "Evaluate").

Try it out!

<div class="sage">
  <script type="text/x-sage">
    @interact
    def _(a=(1, 10)):
      print factorial(a)
  </script>
</div>
