---
layout: post
title:  The pwn experience
date:   2025-02-10
last_modified_at: 2025-02-10
categories: [pwn]
---

Across the last few weeks, I had to build challenges as part of my final project. I had the opportunity to decide and I landed on Binary Exploitation (or pwn).

## Why Pwn?

Not the most ideal way to learn how programs work, but I also wanted to learn how programs can be exploited.

<br>

Prior to the project, I felt that there a lot of gaps in my knowledge of pwn and this was a great opportunity to fill those gaps as much as I could.

<br>

I have never coded in C before, but it was not too hard considering most of the time I could just google up to get a better understanding of the C programming syntaxes.

<br>

**NOTE:** I have only covered some stack-based concepts and techniques.

## The journey

**1. Lack of understanding of the stack memory**

While my school did teach basic data structures (PUSH, POP), it was not really helpful since I was not able to put it to practical use...until now.

To my readers, if you ever attempt to learn pwn, please learn the basics of stack :) It will help you visualise and understand techniques at memory level. Once you able grasp this fundamental well, it becomes a lot easier to learn different techniques.

Really good resource here: [CTF101](https://ctf101.org/binary-exploitation/what-is-the-stack/)

Special thanks to [baesenseii](https://baesenseii.sg/) for clearing my misconception of the stack frame.

**2. Different glibc versions**

Compiling with different `glibc` versions will result in having different set of gadgets and functions. A lot of common gadgets like `pop rdi ; ret` are removed in the newer versions of `glibc`, which was something I did not know at that point in time. This also makes certain techniques like `ret2csu` outdated.

This became obvious once I started dealing with **Return-Oriented Programming (ROP).** This is what happens when you try to look for gadgets you can typically find from other programs.

![glibc 2.40](/assets/images/pwn-challenges/compile.png)

After not being able to figure out how to get the gadgets I wanted for quite awhile, I went to [Elma](https://blog.elmo.sg/) who is an expert at pwn.

![Advice](/assets/images/pwn-challenges/elma.png)

**Solution**

I actually tried both methods. For inline assembly, you can program your code to add gadgets like this:

```c
void gadget() {
    __asm__ volatile (
        "pop %rdx\n\t"  
        "ret\n\t"       
    );
}
```

This adds `pop rdx ; ret` to the program.

For older compilers, I used Docker to install `Ubuntu 18.04` image. IT does not come with `gcc` so I downloaded it and saved this container as a new image so I do not need to install `gcc` everytime I use the image. I also utilised volume mounting which allowed me to directly compile C programs on the host.









