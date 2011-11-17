---
title: Basic Malloc Implementation
layout: post
---

There are a lot of different, heavily-optimized `malloc` implementations, including [tcmalloc](http://goog-perftools.sourceforge.net/doc/tcmalloc.html), [ptmalloc](http://www.malloc.de/en/), [dlmalloc](http://g.oswego.edu/dl/html/malloc.html), and [jemalloc](http://www.canonware.com/jemalloc/) but, before diving into any of those, it's useful to consider how you might implement a very basic `malloc`.

Now, trivially, we could implement `malloc` so that a call to `malloc` basically maps to the system call `sbrk`. `sbrk(n)` moves the location of the *program break* -- the end of the process's data segment -- by `n` bytes, which means that `n` bytes of memory have been allocated to the process.

Our implementation would basically look something like this:

{% highlight c %}
void* malloc (unsigned n) 
{
    return sbrk(n);
}
{% endhighlight %}

However, calls to `sbrk` are expensive. It would be more efficient if our `malloc` implementation allocated a large chunk of memory via a call to `sbrk` and then broke that into smaller pieces when necessary, instead of calling `sbrk` whenever memory needs allocated. 

Keep in mind, though, that -- when there is no memory available -- `malloc` will have to make a call to `sbrk` and, since `malloc` is not the only reason to call `sbrk`, the new chunk of memory will not be contiguous with the old chunk. 

In addition, we will want to re-use any memory that has been free'd, so our `malloc` implementation should keep track of all the memory that's currently available to the program. Since these pieces of memory might not be contiguous, we'll use a linked-list to keep track of them.

Finally, we'll need to keep track of how large each piece of memory in our linked-list is. We can add a `size` field our linked-list struct to do this.

So, putting it all together, this is how our basic `malloc` implementation looks:

{% highlight c %}
#define MINIMUM 1024 /* minimum allocation via sbrk */

struct header {
    struct header* next; /* ptr to next in list */
    unsigned size;
}

static header base; /* list to get started */
static header* freep = NULL; /* start of free list */

void* malloc (unsigned n) 
{
    header* p, *prev;
    unsigned nunits;
    
    nunits = (n + sizeof(header) - 1) / sizeof(header);
    /* check if there's a list of free memory yet */
    if ((prev = freep) == NULL) {
        base.next = freep = prev = &base;
        base.size = 0;
    }
    
    for (p = prev->next; ; prev = p, p = p->next) {
        /* is free piece big enough? */
        if (p.size >= nunits) { 
            if (p->size == nunits) /* exactly? */
                prev->next = p->next;
            else { /* allocate part of free piece */
                p->size -= nunits;
                p += p->size;
                p->size = nunits;
            }
            freep = prev;
            return (void *)(p + 1);
        }
        
        if (p == freep) /* wrapped around list */
            if ((p = moremem(nunits)) == NULL)
                return NULL; /* no free memory */
    }
}

/* request more memory from kernel */
header* moremem (unsigned n) 
{
    char* p;
    header* up;
    
    if (n < MINIMUM)
        n = MINIMUM;
        
    p = sbrk(n * sizeof(header));
    
    if (p == (char *) -1) /* no free memory */
        return NULL;
        
    up = (header *) p;
    up->size = n;
    free((void *)(up + 1));
    
    return freep;
}

{% endhighlight %}

That's the gist of it. I'm not going to bother implementing `free(n)`, but all it does is find where the proper place for `n` is in the list `freep` and then inserts it there.
