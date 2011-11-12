---
title: Operating System Oughts
layout: post
---

Reader mail! Nick Mudgen sent me an email, asking:

> If you were going to create a new operating system, how would it be?

This is a question that all potential OS implementors must consider before setting out to build an operating system. What is quality, really? What makes one thing good and another bad? The answers to these questions will inform your operating system's philosophy. You may want to read the [philosophy behind Unix](http://en.wikipedia.org/wiki/Unix_philosophy).

Here's my first attempt at defining an operating system philosophy. The first rule is more important than the second and so on.

1. **An operating system ought to empower its user.** Computers are tools. The purpose of a tool is to enable humans to accomplish things that would otherwise be impossible.

1. **An operating system ought to be correct.** An operating system should be correct, as robustness is a child of correctness and an operating system must be robust. 

2. **An operating system ought to be simple.** Both an operating system's interfaces and implementation should be simple. However, simple interfaces are more important than a simple implementation.

4. **An operating system ought to be flexible.** The components that comprise an operating system should be easily replaced and modified. (For some interesting research on building flexible operating systems, check out [this paper](http://www.usenix.org/event/usenix09/tech/full_papers/lohmann/lohmann.pdf).)
