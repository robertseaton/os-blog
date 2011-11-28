---
title: Second Week of Plan 9
layout: post
---

I told you that I'm going to [use Plan 9 for one month](http://os-blog.com/one-month-of-plan-9/) and blog about it each week. Another week has passed, so an update is due.

This week I got around to actually using the system after [last week's grueling setup](http://os-blog.com/first-week-of-plan-9/). Most of the work I've been doing lately has been writing prose rather than code, so that's the kind of work I've been using Plan 9 for.

The first thing I noticed about [Plan 9](http://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs) is that, when I'm using it, I'm cool. It's kind of like smoking a cigarette, only for asthmatic Unix geeks. (On some level, it's gotta be unhealthy to mix what operating system you use in with your self-worth but, hey, I'm not a therapist.)

### Acme and Sam

![Plan 9's sam text editor](http://os-blog.com/img/sam_text_editor.jpg)
<div id="credit">
Plan 9's sam text editor
</div>

Plan 9 ships two text-editors, acme and sam. I've been writing exclusively in acme, mostly because acme has a lower barrier to entry. You can open acme and start typing, while I have no idea how to interact with sam without looking at the man page. 

By default, acme wraps lines mid-word and, as far as I can tell, doesn't support word wrapping, a feature I've come to expect from my text editor. I thought that might turn out to be a deal-breaker but, after using acme for a while, it's just an annoyance.

Acme doesn't support syntax highlighting, but I haven't done any programming in the past week, so it hasn't mattered. Syntax highlighting may not be necessary and, sure, the implementation might be hairy but, c'mon, it *makes you smarter.* I can't back that up, but there's a part of the human brain the handles color recognition and, damn it, I want to use it. I miss emacs. 

### Plan 9 and the rest of the world

After writing last week's article inside my Plan 9 VM, I had a problem: I needed to get the post off of the VM and to my MacBook so I could publish it. 

I started with the obvious: ssh.  I'd just scp the file onto my host machine, which was already running an ssh server, so problem solved, right? Wrong. 

Unfortunately, Plan 9 only supports v1 of the SSH protocol, and the default OS X ssh server only allows v2. I realized then that there might be another solution: I could run an http server on the Plan 9 machine and serve up my article to the host-machine. This seemed more appealing messing with SSH, probably because a web server enables all kinds of possibilities.

After a little googling, I had my web server running. It was painless. No messing with config files, nothing. Just run `ip/httpd/httpd` and /usr/web will be served up, Plan 9-style. I downloaded the article and published it, problem solved, crisis averted. 

### Browsing the web

![Abaco's rendering of NYTimes homepage](http://os-blog.com/img/abaco_nytimes.jpg)

<div id="credit">
NYTimes homepage rendered by abaco
</div>

When I'm writing something, I do a fair amount of research on the web so, while I was in Plan 9 this week, I often foud myself cmd+tabbing back to OS X to use Google. This was cheating. 

While researching my web browsing options, I came across this gem:

> The best way to get a fully supported web browser under plan9 is to use the vnc server to connect to a linux or windows box.

That would also be cheating. I went with abaco, since it ships with Plan 9, which meant I didn't need to mess around with installing anything.

Abaco works okay. It's not pretty, but it gets the job done. (Speaking of not pretty, the default font on Plan 9 is not so nice. There's a package to convert TTF fonts to Plan 9 fonts, but I haven't tried it yet.)

### Using the mouse

Getting work done in Plan 9 requires a 3-button mouse. (Well, you can use shift+click as a poor man's third button, but a 3-button mouse is recommended.) Getting work done in Plan 9 requires using the mouse. Want to save a file in acme? Use the mouse. 

This requires a bit of getting used to and it's a far cry from my comfortable Linux desktop where using the mouse usage is entirely optional.

I don't know how I feel about Plan 9's mouse fetish. Whatever the designer's were attempting, it works okay, but I don't feel like there's any obvious win over "traditional" mouse-centric UIs (Windows or OS X) or something more keyboard-driven (emacs). 

Altogether, using Plan 9 this week has been okay. I've managed to get stuff done without relying on emacs or a modern web browser. I don't hate it, but I haven't become a Plan 9 convert either.
