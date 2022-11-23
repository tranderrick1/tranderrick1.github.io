---
title: HackTheBox - The Progression
date: 2022-09-21-2022
categories: [Blog]
tags: [htb]     # TAG names should always be lowercase
author: Derrick
TOC: true
---

![](https://i.imgur.com/1PovHBV.png)

HackTheBox, or HTB for short, is a website that I have used to develop a general penetration testing methodology. The platform provides you with multiple intentionally vulnerable hosts with the intent for you to compromise them. Each machine is effectively a long CTF where you embark on a journey to exploit vulnerabilities to gain flags hidden on the system. Every system provides you with a unique environment designed to expose you to different attack paths in order to teach you without being redundant from an older machine.

## Trust The Process
---
![](https://i.imgur.com/21KySwf.png)

### Testing the waters (Late Fall 2021)
The very first box I took on went by the name "Cap". The mascot was a pirate with a face seemingly saying "Arghh". The box is aptly named cap due to the intended path involving p.CAP files as well as certain binary CAPabilities. Despite the box only requiring roughly 3 steps and the fastest completion time being roughly 5 minutes, it took me over 12 hours to complete. I admit my method for learning was not the greatest and I don't recommend people to bash their head similarly to how I did, but its what I did to learn. I remember staring at my screen for several hours at a time searching up the most random things in attempt to move forward. Funny story, I remember searching up "Port 22 exploit" following an nmap scan as if it would help me. The first handful of boxes took me several hours to complete just a few steps. It was demoralizing I will admit. After 12 hours I looked back and realize how few things I have done. It's painful, but that sound that plays when you submit the root flag, boy does that fuel me for days.

### Taking things seriously (All Summer 2022)
After completing a few boxes of the current rotation, I felt confident enough to purchase a VIP subscription. With this I am now able to access all of the old retired boxes. I tried my hand at several of the old easy and medium boxes and quickly realized that during my head-bashing sessions, I actually obtained a very primitive penetration testing methodology. These old boxes would be completed with ease. I would stay up nights trying to complete as many boxes as I could in a day. The sun peering through my window would by my signal to go to bed. 12+ hours would shrink to 1-3 hours for these quick and easy boxes and pretty soon, I realized, "I should aim for Pro Hacker".

### The push for Pro Hacker (Early Fall 2022)
HTB gives out titles based on completion of the current rotation of boxes. The issue is, there is no telling of how good or bad the quality of these boxes may be. However, having completed 40 boxes told me that I might be ready for the challenge. And so I started, one by one clearing all of the easy boxes. Took a bit but that was not too tough. Then came the medium boxes. I would spend a handful of hours across these boxes. One in particular I was not even able to compete. A box named "Retired" dealt with a buffer overflow in which I have no idea how to perform on a Linux system. Luckily for me, the box was retired (haha) and replaced by a much more reasonable  "medium" box. Upon completing all of the easy and medium boxes, I only need 3 and a half more boxes to gain the elusive title. Issue is, only insane and hard boxes remain. Taking my brother's advice, I decide not to even consider any of the insane boxes as those would put me through utter hell. That leaves only hard boxes left. I did some background research to complete the easier hard boxes and really, they were just longer versions of an easy or medium box. Not too bad. The two boxes left in my way were "Carpediem" and "Vessel". These two boxes were rated pretty highly by the community. Accompanying the high ratings were also the high difficulty ratings. Long story short, I managed to overcome the both of them to gain my title of "Pro Hacker". The cost? Only about 50 hours over the course of 2 weeks. I have detailed writeups on my notes which I will link at the bottom of the page if you are interested.

---

![](https://i.imgur.com/oqU14hL.png)

After obtaining the title, a couple of my colleagues along with my brother convinced me to move on from the platform if I wanted to continue my learning. While it is true that I learned a lot from HTB, I was also becoming comfortable. I was performing different attacks on each machine however, my methods and tooling were always the same. If I want to improve technically, I need to start reading into how XYZ works or prepare for OSCP/CTRO. Regardless of what's next, I need to get ready for CPTC. Western Regionals are coming up.

## What I Have Learned
---
![](https://i.imgur.com/z4CILFW.png)
HTB built my technical skills to where they are currently as of the time of writing. I first got started with these boxes back in August 2021 but did not actually take it seriously until about April 2022. Within the 5 months I've taken the platform seriously, I saw my box completion times drop from around 24h+ to around 2-4h depending on the length and difficulty of the machine. I remember spending hours trying to detect and perform a simple SQL injection on a website but now its part of standard procedure. Every challenge along the way slowly became integrated into my bag of tools to toss at a lock in the future.

One of my greatest takeaways from HTB is the importance of debugging. What really separates a lethal penetration tester from an average one is, in my opinion, their debugging skills. It's one thing to be technically sound and know the ins and outs of an attack vector but that does not scale well in the grand scheme of things. Being able to use your current knowledge to adapt and overcome challenges through debugging the source of error within your method is much more important. I can recall a handful of times where I have gotten stuck at some wall where I am unfamiliar with the intended attack path only for my younger brother to swoop in and give it a shot. Despite me having full context of the environment up until that point, my younger brother can debug my source of error and solve the issue with lightning speed. I vividly remember him complaining at how bad my keyboard is as he stumbled across which keys he wanted to press. As I stare at the console, I can hardly keep up with his commands and barely understand what is going on. When I concentrate hard enough, I can see that he is trying to understand at what point is the attempt failing and what other options does he have? 95% of the time this process eventually leads to success and opens the path further into the environment. In short, debugging is a critical skill to have and the better you are with it, the more well off you are.

## How I Use HackTheBox Nowadays
---
<img src="https://i.imgur.com/D5mn64D.png" width="400px">


Recently I have moved away from HTB to focus more on other projects or competitions that I'm gearing up for (CPTC and RvB!). HTB is great for learning technical skills however it somewhat caused me to develop tunnel vision. HTB's environments tend to have one answer and the unintended paths tend to appear from the age of the machine or are some crazy high level find that I wont ever reach. Instead of treating HTB as if it was a realistic environment, I now use it as more of a sandbox. Setting up my own home lab can be tedious so why not use publicly available ones to test attacks instead? It is a trade off customizability for convenience. One of these days I'll set up my own home lab as that alone will teach myself plenty of things but for now I'll be using the "prebuilt" environments on HTB instead. No longer is HTB a puzzle or game where I would derive a sense of accomplishment by figuring out the intended attack path, but rather I use it to learn about why certain attack vectors work at a deeper level. It's fun to test new tools on machines rather than going in blindly to figure something out after several hours of head-bashing.

## My Personal Notes
---
I've taken down the posts that were originally on this site. As I realized, the more boxes I do the more I'll flood this page up. Instead I have decided to publicly host my notes on Notion instead and link it [here](https://succulent-lentil-32e.notion.site/HackTheBox-af043e338cd14648aa702945f127bae0). Also if anyone is interested in my cracked younger brother and what he is up to, check out his [blog](https://dtsec.us). Some pretty crazy stuff.