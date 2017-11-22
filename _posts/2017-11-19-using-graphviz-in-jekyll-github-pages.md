---
title: Using graphviz in Jekyll Github pages
graphviz: true
---

Graphviz is a tool that allows you to draw graphs with code. With some straight forward statements like: 


{% highlight javascript linenos %}
digraph G {

    subgraph cluster_0 {
        style=filled;
        color=lightgrey;
        node [style=filled,color=white];
        a0 -> a1 -> a2 -> a3;
        label = "process #1";
    }

    subgraph cluster_1 {
        node [style=filled];
        b0 -> b1 -> b2 -> b3;
        label = "process #2";
        color=blue
    }
    start -> a0;
    start -> b0;
    a1 -> b3;
    b2 -> a3;
    a3 -> a0;
    a3 -> end;
    b3 -> end;

    start [shape=Mdiamond];
    end [shape=Msquare];
    }
{% endhighlight %}

Graphviz would turn the code into a graph like this for you:

{% capture foo %}
    start -> a0;
    start -> b0;
    a1 -> b3;
    b2 -> a3;
    a3 -> a0;
    a3 -> end;
    b3 -> end;

    start [shape=Mdiamond];
    end [shape=Msquare];
{% endcapture %}

{% include graphviz.html
    title="G"
    type="digraph"
    graph=foo
%}

This is very sweet! 
I learnt of the convenience of graphviz from [HackMD](https://hackmd.io)'s [features page](https://hackmd.io/features), and I could immediately see the value having such a tool in blogging, documentation, or just any writing.
I want it on my blog project, after all, this blog is about hacking, or doing things the smart way. This post is about how I got it to work on a Jekyll website. Although there's two options listed below, the github page option would 

Self-hosted option:
* Graphviz is actually a command line program, that honors the DOT syntax (the syntax )
At first I thought the syntax is native to markdown, since HackMD is a markdown based knowledge collaboration platform.
But pasting the code directly from HackMD to my Jekyll based blog (here) didn't work. Jekyll, or the Liquid templating language for that matter, or both, doesn't recognize the tag.

As it turned out, graphviz Surprisingly, there weren't many discussions on graphviz. 



graphviz

Github page option:
First of all, lets try to see if the JS file is correctly loaded.
We then have to make sure other things.

To be comprehensive, let's create a check list:

To make the code available:
- [ ] JS file must be put into assets/js/vendor/vizjs/viz-lite.js
- [ ] That file has to be included in _includes/head/custom.html

To create those 
- [ ] 

{% include graphviz.html title="simple" type="digraph" graph="a->b" %}
