---
title: 2022 CPP Cyber Bootcamps
date: 2022-08-27-2022
categories: [Blog]
tags: [cptc,ccdc,competitions]     # TAG names should always be lowercase
author: Derrick
TOC: true
---

![](https://i.imgur.com/OT9eKFl.png)

CPP Cyber is the competition team here at California State Polytechnic University, Pomona. Only skilled individuals are allowed to make it onto the team and CPP Cyber helps you with that by hosting these rigorous bootcamps to teach you the skills necessary to join the team. I have participated in the bootcamp in the year prior however, I did not take it seriously and paid the consequences by not making it onto either of the teams. This year, I decided to change that and make at least one of the teams.

The bootcamp is marketed to be beginner friendly but to be honest, it isn't really. The bootcamps are quite difficult and require you to go above and beyond outside of the allocated training times. This bootcamp is not for the faint of heart.

## Prior to the Summer
---
![](https://i.imgur.com/QpaITDG.png)
Shortly after ITC and RvB took place on the same day back in April, my schedule was clear to study up for the bootcamps. I am very inexperienced when it comes to the field of cyber, but I have a keen interest in it. Most of the people on either of the teams have several years of experience. Heck even some of the people are working in the industry already. Meanwhile there's me, barely managed to exit VIM a few weeks ago. I have a lot to do if I want to make any of the teams.

## HTB Arc
---
![](https://i.imgur.com/sxh1Hn1.jpg)
I'll keep things short so that I do not have to repeat myself so much from my upcoming blog post however, here is the short version. CPTC was my main priority so immediately I set out to get started in my hacking journey. I have primarily used a platform called HackTheBox. Their website contains multiple environments that are vulnerable so that players such as me can attempt to fully compromise the system. By the time the tryouts for the CPTC came around, I had completed just under 40 boxes. I would like to think that after all of that, I am slightly prepared for what's next to say the least.

I'm currently working on another blog post regarding my HTB journey so give that a read if you get the change to. I plan to fully flesh out my experience with HTB when I reach a nice ending point, so to speak.

## Bootcamp Officially Starts
---
![](https://i.imgur.com/4rP3HeX.png)


Luckily for us this year, the bootcamp was held in-person along with a hybrid component for our online zoom buddies. Bootcamp was held every Saturday from, if I recall correctly, 10am-4pm. 10am - 12pm was the time slot for CPTC and 1pm-4pm was for CCDC. I showed up in person every single week that I could and gave it my absolute all. In contrast to the previous year, I would actually pay attention to the presenters and not sleep. The only issue was that for CPTC, I already knew roughly 75% of the material that they were teaching. Another issue was that once CCDC started, I was solely focused on doing CPTC homework. CCDC topics were interesting but the homework was nothing short of brutal. Giving beginners one week to set up Wordpress on IIS among other hellish tasks is a crime. Many tears were shed while trying to manually set up PHP for the first time. Not mine of course.

I had to cut the bootcamp short due to a family vacation. I missed most of the material from week 4-6 and my grades reflected that. Upon returning, my body was still in vacation mode and I was just on cruise control all the way until the tryouts. A little bit unfortunate but hey, I had my fun.

## The Tryouts
---
![](https://i.imgur.com/ftOG5uH.png)

### CPTC
CPTC tryouts were the first of the pair to roll around and boy was I more than ready. Prior to the tryout, I ran through the previous year's environment and pwned it completely, from the inside out. Last year when I tried out, I'd say I completed about less than 10% of it. To say the least, I was extremely confident on the day of the tryout. The moment it began, I performed an nmap ping sweep to identify the hosts. 3 targets, the dot 3, dot 5, and dot 10. Quickly pull up tmux and split into 3 new windows each with 2 separate panes. One pane was for a quick full port scan while the other was for a service version and script scan. Within 5 minutes I already had my targets fully scanned. Within 1 hour, the Linux host was already compromised. Within the next hour both hosts have fallen. By the time I moved onto the third, I realized I made a fatal error. To replicate the infamous port 502, modbus for typical ICS software, the dev team made the final box purposefully fragile. My "-sVC" scan tore it down and rendered it inaccessible. That's a big whoops! Totally didn't cry myself to sleep for a few hours.

The following day I went down to a friend's house to dine on some nice A5 Wagyu steaks. Needless to say, my report was not shaping up. I got home and my fingers never left my keyboard. I wrote everything down as fast as I could, modeling my report after RIT's 2020 global report. In the end, I got the highest report score of 77.5/100 clearing the second place score of 62. Most people got in the 20s-30s. The standouts got 50+. The scoring team was pretty brutal. But that is only half the journey, CCDC is next.

### CCDC
As for the more prestigious competition, you would think that I would try to take more care for it, but I was quite burnt out by then. Riding off my high of the results from CPTC tryouts, I couldn't really care less for CCDC. I kind of just winged it. This past RvB, a competition hosted by SWIFT modeled after CCDC, a fellow competitor and I managed to get first place. As a team of 2 edging out every other team of 4, we were pretty well set off. Going into the tryouts, I fumbled the bag, HARD.

There were some bumps during the tryouts such as I didn't actually know my team number until half way into the competition. All the services that I thought was up was not actually up. I wasn't in the top 10, I was straight up in the bottom 10. By the end of the scoring checks, I wasn't even in the top half of the competitors. I utteraly flunked it.

Since my younger brother was already on the CCDC team and one of the scorers, I was slightly eavesdropping on his room to get the results prematurely. I heard them say something along the lines of: "Oh he isn't doing too well. Oh shit, his service points aren't great at all, he's not going to make it. Wait, put in the inject scores, let's see. Oh? WAIT HE'S GONNA MAKE IT!? Put in the homework scores quick! Oh wait nevermind he's out of the race." My carelessness on the CCDC homework cost me my spot on the team. Oops! I'll redeem myself next year.

## Looking Forward
---
![](https://i.imgur.com/FH1Q9Bq.png)
All in all, the bootcamp was an amazing experience. Getting to see my fellow competitors aiming for a spot on the team sitting side by side with me in the room was quite fun. Really got my blood boiling from all the excitement. The next major milestone in my journey now is the CPTC Western Regionals in November. Look forward to that post after CPTC is all said and done.