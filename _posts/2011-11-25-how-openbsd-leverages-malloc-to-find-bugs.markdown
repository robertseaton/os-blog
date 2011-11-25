---
title: How OpenBSD Leverages Malloc to Find Bugs
layout: post
---

![OpenBSD Carved Pumpkin](http://os-blog.com/img/openbsd_pumpkin.jpg)

I was reading about different `malloc` implementations on [this Wikipedia article](http://en.wikipedia.org/wiki/Memory_management) when I stumbled upon an interesting tidbit regarding OpenBSD's memory management strategy.

It turns out, [OpenBSD](http://www.openbsd.org/) implements `malloc` in a certain way so that programs that try to write to free'd memory will segfault (crash). The advantage of this is that it makes this entire class of bugs painfully obvious to the user. As far as I know, this property is unique to OpenBSD's memory allocator.

### How it works

For an example of why a `malloc` implementation might operate in such a way that writing to free'd memory doesn't immediately result in a segfault, you can check out my [example malloc implementation](http://os-blog.com/basic-malloc-implementation/).

OpenBSD operates differently. On OpenBSD, when a program requests more memory than the system's page size, it's allocated via a call to `mmap`. In this case, `mmap` is being used to map part of main memory into the program's address space. If the program requests less memory than the system's pagesize, it's allocated from buckets set up earlier in the program's execution. These buckets are also allocated via `mmap`.

Since calls to `mmap` on OpenBSD are randomized, this has some security benefit.

The important bit, however, is how OpenBSD's libc handles calls to `free`. Rather than cache the free'd memory so it can be reused, `free` immediately unmaps the memory from the program's address space via a call to `munmap`. Thus, if the program tries to write to that memory after calling `free`, it will segfault.

### The philosophy behind OpenBSD

The most interesting part of this is the trade-off that the implementors were forced to make. Rather than cache free'd memory to improve performance, they chose to craft the implementation so that incorrect programs will crash. 

One of the guiding principles of the OpenBSD project is security, so this trade-off makes sense. A program that tries to use free'd memory is probably a program that can be exploited.

What I really love about this, though, is that the OpenBSD developers have chosen to design their operating system in a way that exposes an entire class of flawed programs. That's really kind of empowering. If your program segfaults on OpenBSD, you probably have some bugs to fix.
