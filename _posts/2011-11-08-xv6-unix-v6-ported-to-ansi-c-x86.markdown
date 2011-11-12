---
title: "Xv6: Unix v6 Ported to ANSI C, x86"
layout: post
---

Russ Cox, Frans Kaashoek, and Robert Morris at MIT came across certain challenges while introducing students to the Unix sources in 6.828: Operating Systems Engineering. Although the Unix v6 source may seem like an ideal introduction to operating systems engineering because of its simplicity, students doubted the relevance of an obsolete OS written in a now defunct dialect of C. In addition, students struggled to learn the details of two different architectures, the PDP-11 and x86, at the same time.

So, in typical MIT fashion, they wrote a new OS to replace Unix v6 in 6.828. Dubbed xv6, it's written in ANSI C and runs on Intel x86 machines. It's not a straight port, since code had to be added to handle concurrency, and the rougher parts of v6 (i.e. the scheduler and file system) were replaced with cleaner versions. The entirety of xv6 weighs in at less than 9,000 lines of code.

More info on the project can be found [here](http://pdos.csail.mit.edu/6.828/2011/xv6.html).

To checkout the source:
{% highlight console %}
git clone git://pdos.csail.mit.edu/xv6/xv6.git
{% endhighlight %}

