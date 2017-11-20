---
title: Using graphviz in Jekyll Github pages
graphviz: true
---

First of all, lets try to see if the JS file is correctly loaded.
We then have to make sure other things.

To be comprehensive, let's create a check list:

To make the code available:
- [ ] JS file must be put into assets/js/vendor/vizjs/viz-lite.js
- [ ] That file has to be included in _includes/head/custom.html

To create those 
- [ ] 

{% include graphviz.html title="simple" type="digraph" graph="a->b" %}
