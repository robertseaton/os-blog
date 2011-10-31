---
title: "x86_64 Assembly Language Tutorial: Hello, World!"
reddit_community: programming
layout: post
published: false
---

![Man standing at blackboard.](http://os-blog.com/img/blackboard.jpg)

If you're going to build your own operating system (and you *are* going to, right?), you'll need to know some assembly language. Once you know assembly language, you might even choose to write your [entire OS in assembly](http://www.returninfinity.com/baremetal.html). Whatever you choose, this tutorial will gently introduce you to the x86_64 assembly language, while future installments in the "x86_64 Assembly Language Tutorial" series will cover progressively more advanced topics.

To receive notifications when more of the "x86_64 Assembly Language Tutorial" series is published, you should subscribe to our [RSS feed](http://feeds.feedburner.com/os-blog/H) or [email newsletter](http://eepurl.com/gIQ-P).

### What You'll Need

Before we can get started, you're going to need an x86_64 Linux machine with the program nasm installed. You should be able to find nasm in your distribution's repository. 

If you don't already have a Linux machine, you can follow our [guide to installing Linux in a virtual machine](http://os-blog.com/installing-a-linux-virtual-machine).

### "Hello, World!"

Like most programming language tutorials, we will start by crafting the most basic of programs: "Hello, World!" I will start by showing you the code, which I recommend you type in manually -- no copy and paste! -- to better commit it to memory. 

First, though, let's create a directory to store our work in.

{% highlight console %}
$ mkdir asm-tutorial
$ cd asm-tutorial
$ gedit hello-world.asm
{% endhighlight %}

In the above example, I'm  opening the file hello-world.asm with gedit, an easy to use, general purpose text-editor, but if you would rather use emacs, vim, or some other text-editor, feel free to do so. 

Now, let's type in the code for our "Hello, World!" program. I will explain how it works after you've managed to successfully compile and run it. 

{% highlight nasm %}
[bits 64]
	global _start

	section .data
	message db "Hello, World!"

	section .text
_start:
	mov rax, 1
	mov rdx, 13
	mov rsi, message
	mov rdi, 1
	syscall

	mov rax, 60
	mov rdi, 0
	syscall
{% endhighlight %}

### Creating an Executable

Once you are done entering this, save the file and then type the following into your terminal:

{% highlight console %}
$ nasm -f elf64 hello-world.asm
$ ld hello-world.o -o hello-world
$ ./hello-world
Hello, World!% 
{% endhighlight %}

The first command, `nasm -f elf64 hello-world.asm`, tells the program nasm to assemble our file. `-f elf64` specifies that the we want nasm to produce an object file of the elf64 format. 

nasm is what is known as an assembler. An assembler converts a file written in assembly language, like hello-world.asm, into opcodes. Opcodes tell the machine what operation to perform. The files produced by nasm are called object files. In our case, nasm produced one file, hello-world.o.

We can check out what our object file contains using the hexdump utility.

{% highlight console %}
$ hexdump hello-world.o
...
0000080 0001 0000 0001 0000 0003 0000 0000 0000
0000090 0000 0000 0000 0000 0200 0000 0000 0000
00000a0 000f 0000 0000 0000 0000 0000 0000 0000
00000b0 0004 0000 0000 0000 0000 0000 0000 0000
00000c0 0007 0000 0001 0000 0006 0000 0000 0000
00000d0 0000 0000 0000 0000 0210 0000 0000 0000
...
{% endhighlight %}

As you can see, machine opcodes are intended to be read by computers, not human beings. 

The final step that we must take before we can run our program is called linking. It's performed by the system's linker, which -- on Linux machines -- is called ld. Linking is the process by which object files are combined and transformed into an executable.

The `-o hello-world` option that we passed to ld tells ld that we want our executable to be named hello-world.

Finally, we run the executable that we produced by prefixing the filename with "./". Your program should reward you with "Hello, World!"

### A Word on CPU Operation

Before we start digging into the source code of our "Hello, World!" program, it's useful to have a rough mental model of how the CPU operates. Fundamentally, a CPU's purpose is to execute a sequence of stored instructions, a program. There are four steps that CPUs use in their operation: fetch, decode, execute, and writeback.

1. **Fetch**

    The first step, fetch, involves "fetching" an instruction from program memory. The location in memory of this instruction is determined by the program counter, which stores the current position of execution in the program. Once an instruction is fetched, the program counter is incremented so that it points to the next instruction.

2. **Decode**

    Decode, determines what operation the CPU will perform. The instruction is broken into parts: the opcode, which indicates which operation to perform, and the remaining parts provide information required for that operation, either a constant value, a register, or a location in main memory.

3. **Execute**

    After the decode step, the execute step is performed. Various portions of the CPU are connected so that they can perform the operation specified by the opcode and then the operation is performed.

4. **Writeback**

    The final step, writeback, simply stoes the result of the execute step to some form of memory: a register or a memory address. Not all instructions produce an output, some manipulate the program counter. These instructions are called jumps, and they facilitate stuff like loops, conditional statements, and function calls.

### Understanding Registers

A processor register is a small amount of storage that is part of the CPU. There are three types of registers that we are primarily concerned with: data registers, address registers, and general purpose registers. 

* **Data registers** hold numeric values (such as integers or floating point values).

* **Address registers** store addresses of locations in memory. 

* **General purpose registers** can act as either a data register or an address register. 

Much of the work of an assembly programmer is concerned with manipulating these registers.

### Analyzing Our Source Code

With that background out of the way, we can dig into our program's source code. I will break it down piece by piece and, along the way, explain what each step accomplishes.

{% highlight nasm %}
[bits 64]
{% endhighlight %}

The first line of our "Hello, World!" program  is an assembler directive. Assembler directives modify the behavior of the assembler instead of being translated to machine code. The `[bits 64]` directive, as you may have guessed, tells nasm that we want to generate code designed to run on a processor operating in 64-bit mode.

{% highlight nasm %}
    global _start
{% endhighlight %}

This line is another assembler directive. It tells nasm that the section of our code marked with `_start` should be considered global, which typically allows for other object files to refer to it. In our case, we mark the `_start` section as global so that the linker knows where our program begins.

{% highlight nasm %}
    section .data
    message db "Hello, World!"
{% endhighlight %}

The first line here is another assembler directive. It tells nasm that the code that follows it is part of the data segment. The data segment contains global and static variables. 

Next, we have one of those static variables: `message db "Hello, World!"`. `db` is used to declare initialized data and `message` is a variable name that will be associated with `"Hello, World!"` 

{% highlight nasm %}
    section .text
{% endhighlight %}

Now, we have another `section` directive, but this time it's telling nasm to store the code that follows it in the text segment. The text segment, also sometimes called the code segment, is the section of the object file which contains executable instructions. 

Finally, we come to the meat of our program:

{% highlight nasm %}
_start:
	mov rax, 1
	mov rdx, 13
	mov rsi, message
	mov rdi, 1
	syscall

	mov rax, 60
	mov rdi, 0
	syscall
{% endhighlight %}

The first line, `_start:`, associates the symbol `_start` with the portion of code that follows it.

{% highlight nasm %}
	mov rax, 1
	mov rdx, 13
	mov rsi, message
	mov rdi, 1
{% endhighlight %}

These next four lines all load values into different registers. Register RAX and register RDX are both general purpose registers. We are using them to hold 1 and 13, respectively. Register RSI and register RDI are source and destination data index registers. We're setting the source register (RSI) to point to `message` and the destination register (RDI) to 1.

Now, after loading up these registers, we have `syscall`, which tells the computer that we want to execute a system call using the values that we loaded into the CPU's registers. The first number that we loaded, the one in register RAX, tells the computer what syscall that we want to use. A table of syscalls and their respective numbers is available [here](http://lxr.free-electrons.com/source/arch/x86/include/asm/unistd_64.h).

As you can see from the table, the 1 that we loaded into register RAX means that we will be calling `write(int fd, const void* buf, size_t bytes)`. The first number we loaded after loading register RAX, `mov rdx, 13`, is the same number that will be used for the final parameter of the `write()` function. The final parameter of `write()`, `size_t bytes`, specifies the size of our intended message and, indeed, the value in RAX, 13, is the length of our message, "Hello, World!"

The next two loads, `mov rsi, message` and `mov rdi, 1` cover the next two arguments, respectively. So, if we put it all together, by the time we get to `syscall`, we are telling the computer to execute `write(1, message, 13)`. 1 is the same as stdout, so essentially we are telling the computer to write 13 bytes from `message` to stdout, which is exactly what our program does.

{% highlight nasm %}
	mov rax, 60
	mov rdi, 0
	syscall
{% endhighlight %}

Now, you're probably wondering why there is another `syscall` when we have already covered all of our program's functionality. You don't have to guess at what it does, though, we can just look up what syscall the number in RAX, 60, refers to from [the aforementioned table](http://lxr.free-electrons.com/source/arch/x86/include/asm/unistd_64.h). As you can see, 60 refers to `exit()`, so this last bit of assembly code is calling `exit(0)`. 

### Putting It All Together

So, the first part of our assembly program associates the variable `message` with `"Hello, World!"` Then, the meat of our program makes two syscalls, one to `write()` and one to `exit()`. Putting it all together, if we were to translate our assembly program into C, it would look something like this:

{% highlight c %}
int main() 
{
	char* message = "Hello, World!"
	
	write(1, message, 13);
	exit(0);
}
{% endhighlight %}

