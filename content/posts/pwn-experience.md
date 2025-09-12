---
layout: post
title:  The pwn experience
date:   2025-02-10
last_modified_at: 2025-02-10
categories: [pwn]
---

Across the last few weeks, I had to build challenges as part of my final project. I had the opportunity to decide and I landed on Binary Exploitation (or pwn).

## Why Pwn?

Prior to the project, I had some idea about pwn but felt that there a lot of gaps in my knowledge and this was a great opportunity to fill those gaps as much as I could.

I have also never coded in C before, but it was not too hard considering most of the time I could just google up to get a better understanding of the C programming syntaxes.

Simply put: I was not good at Pwn. 

But every time I say I am not good at something, this scene from Haikyuu always pops in my mind:

![](/images/compile.png)

This then got me started.

**NOTE:** I have only covered some stack-based concepts and techniques over the span of a few weeks.

## The journey

### 1. Lack of understanding of the stack memory

While my school did teach basic data structures (PUSH, POP), it was not really helpful since I was not able to put it to practical use...until now.

To my readers, if you ever attempt to learn pwn, please learn the basics of stack :) It will help you visualise and understand techniques at memory level. Once you are able grasp this fundamental well, it becomes a lot easier to learn different techniques.

Really good resource here: [CTF101](https://ctf101.org/binary-exploitation/what-is-the-stack/)

Special thanks to [baesenseii](https://baesenseii.sg/) for clearing my misconceptions.


### 2. Different glibc versions


Compiling with different `glibc` versions will result in having different set of gadgets and functions. A lot of common gadgets like `pop rdi ; ret` are removed in the newer versions of `glibc`, which was something I did not know at that point in time. This also makes certain techniques like `ret2csu` outdated.


This became obvious once I started dealing with **Return-Oriented Programming (ROP).** This is what happens when you try to look for gadgets you can typically find from other programs.


![glibc 2.40](/images/compile.png)


After not being able to figure out how to get the gadgets I wanted for quite awhile, I went to [Elma](https://blog.elmo.sg/) who is an expert at pwn (Big thank you). 

![](/images/elma.png)

### Solution

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


For older compilers, I used Docker to install `Ubuntu 18.04` image. It does not come with `gcc` so I downloaded it and saved this container as a new image so I do not need to install `gcc` everytime I use the image. I also utilised volume mounting which allowed me to directly compile C programs on the host VM.

### 3. Buffering

When I started testing challenges on remote (Docker), there was no output from my program although I had connected to it. 

While this problem also took time, I was able to figure out that it was due to the buffering involved in C programming. This meant that the program output was stored in buffer temporarily until I made an input.

This snippet of code helps to disable buffering, which ensures all I/O & errors are shown immediately:

```c
setvbuf(stdout, NULL, _IONBF, 0);
setvbuf(stdin, NULL, _IONBF, 0);
setvbuf(stderr, NULL, _IONBF, 0);
```

### 4. AYCEP (Div0-N0h4ts)

During my project, I set aside time to attend this programme and it was really meaningful. It was a privilege to be taught more advanced pwn techniques and also got introduced to kernel exploits (which tbh I still haven't got a clue to how to do it but I can do it soon enough).

I actually attended AYCEP twice before this and there seems to be this trend of me getting rekt in finals CTF (3 consecutive AYCEP finals CTF btw).

Being able solve the pwn challenges during qualifiers...and not a single one during finals. Truly a humbling experience.

![](/images/cope.png)


This time not solving meant a lot more to me since I did my best trying to learn. While I did feel defeated, I continued to try and solve the challenge that I could not right even after the CTF ended (it was a `ret2syscall` challenge).

## Closing thoughts

I had a lot of fun learning pwn and building pwn challenges. While I acknowledge that I have a little more experience with pwn now, there is still a lot more to it and I hope to continue to learn from people who are way better than I am while teaching others what I know.


## What's to come for this blog

Just about anything that goes in my infosec life. I'm currently planning to do HackTheBox labs and retake CRTP (Certified Red Team Professional) certification that I failed back in October 2024.

If you've made it this far, THANK YOU FOR READING :D