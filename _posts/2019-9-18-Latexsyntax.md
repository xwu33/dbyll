---
layout: post
title: Notes about Latex and how to use it in Markdown files
categories: [Latex]
tags: [Latex]
fullview: false
comments: false
shortinfo: This post writes some notes about Latex syntax and how to write formulas in Markdown files.
---
#### Enable the LaTex expression in MarkDown files for github page
##### Option 1:
Write your equation in [MathURL](http://mathurl.com/) and embed it.
You could write the equation with MathURL, then generate a url that permanently points to the equation, and display this in an <iframe> tag. However, this will stop working if MathURL goes offline.
##### Option 2:
Implement [jsMath](http://www.math.union.edu/~dpvc/jsmath/)
##### Option 3:
[Mathjax](http://dasonk.com/blog/2012/10/09/Using-Jekyll-and-Mathjax)





#### Latex cheat sheet

#### Latex for fomula
You can use
$$\LaTeX$$
to typeset formulas. A formula can be displayed inline, e.g. $$e=mc^2$$, or as a block:
$$\int_\Omega \nabla u \cdot \nabla v~dx = \int_\Omega fv~dx$$
Also check out this [LaTeX introduction](https://en.wikibooks.org/wiki/LaTeX/Mathematics).
