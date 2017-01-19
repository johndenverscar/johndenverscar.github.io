---
layout: post
title: Map vs. Reduce
date: 2017-01-18 22:23
---

Of the dozens of tools in the functional toolbox, `map` and `reduce` are two of the most important.
I've heard comparisons of `map` and `reduce` to table saws, power drills, laser cutters, 
and light sabers...
In reality, they are simpler. Much simpler.
They are more like a hammer and a chisel.

A hammer and chisel in the hands of an amatuer aren't all that useful.
Really, all that comes of it, is wood chips...
But, if you give them to a master carpenter, they can be used to create art.

Before we continue, I'm going to assume you have at least a general understanding 
of higher order functions.

# Map

There are bunch of definitions of `map` in mathematics and computer science.
For our purposes, a `map` is a higher order function whose parameters are 
a function, f, and a list, [a0, a1, a2, ..., aN].
For every value, v, in the list, map will return, f(v).

Now, that doesn't seem too difficult.
It sounds like a `for` loop or a `for each`.

