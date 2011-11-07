---
title: Better OS-level Abstractions for GPUs
layout: post
---

Microsoft Research, with contributions from researchers from Technion and The University of Texas at Austin, presented a paper titled *PTask: Operating System Abstractions To Manage GPUs as Compute Devices* at the 23rd ACM Symposium on Operating Systems Principles (SOSP), which took place in Portugal on October 23rd through the 26th. You can download the paper free of charge [here](http://research.microsoft.com/apps/pubs/default.aspx?id=154952), while a list of all the papers presented at SOSP can be found [here](http://sigops.org/sosp/sosp11/current/index.html). 

### Background and Motivation

With the advent of general-purpose GPU (GPGPU) programming, the GPU has become a computing resource that, for parallel workloads, rivals and often outperforms CPUs. Outside of the domain of high-performance computing, however, few applications utilize the GPU as a general-purpose computing resource, even though suitable GPU hardware has become commonplace and can be found in many consumer-facing devices. The paper argues that this lack of adoption is not because few problems are inherently parallel, but rather that GPGPU programming places unnecessary burdens on the programmer, which makes GPGPU programming more difficult than it strictly needs to be. In addition, the authors argue, lack of modularity and lack of guarantees about GPU performance hinder widespread adoption.

### The PTask API

The researchers then propose and implement a new, OS-level interface for GPU programming, called the PTask API, which provides a dataflow programming model in which the programmer writes code to manage a graph-structured computation. Dataflow programming, first proposed in [Bert Sutherland's Ph.D. thesis](http://dspace.mit.edu/handle/1721.1/13474) in 1966, focuses on how things connect rather than how things happen. Operations are represented by nodes on a graph, which take explicitly defined inputs and produce explicitly outputs. These nodes are then connected and the computation flows through the graph.

In the paper, the following goals for the PTask API are explicitly defined:
* Bring GPUs under the purview of a single resource manager, allowing the OS to provide meaningful guarantees regarding isolation and fairness.
* Provide a programming model that simplifies the development of code for accelerators by abstracting away code that manages devices.
* Provide a programming model that allows code to be both modular and fast.

### Code

While the paper does describe the PTask API and discuss integrating it with the Linux kernel, the code for their implementation is not provided, which means that it would prove difficult to reproduce the API. I've sent an email to the authors of the paper asking them if they have considered publishing their source code, as I'm certain it would be of some interest to individuals involved with OS implementation. We'll see if anything comes of it.

Check out the paper, though, it's an interesting read.
