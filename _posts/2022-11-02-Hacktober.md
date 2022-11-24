---
title: Hacktober - SWIFT's Month of SCARE
date: 2022-11-02-2022
categories: [Blog]
tags: [swift]     # TAG names should always be lowercase
author: Derrick
TOC: true
---
![](https://i.imgur.com/Lk6FtGh.png)

This past October, SWIFT has celebrated the National Cyber Security Awareness Month by hosting what we call Hacktober! So usually here at SWIFT, we teach all sorts of topics ranging from Windows/Linux security to the basics of the CI/CD pipeline. For October in particular, we focused solely on hacking hence the name, Hacktober. I even got to lead my first SWIFT workshop regarding web applications. Between the 4 weeks, the first 3 would be led by one of the CPP CPTC "Kill Squad" members teaching some topic. The final week capped off with a really cool event called "SCARE" or SWIFT's Crack Attack Root Event.

## The Calm Before the Storm
---
![](https://i.imgur.com/1N88svA.png)

I knew that Hacktober was coming up and I really enjoy teaching topics related to offensive security. This would be a great opportunity for me to spread the knowledge. The only issue is that I have not given a formal presentation in a few years so, here goes nothing, right? The next question was, "What do I want to teach?". List of topics to cover over the month:

<table style="margin-left:auto;margin-right:auto">
    <thead>
        <tr>
            <th style="text-align: left;padding: 0.4rem 1rem">Topic</th>
            <th style="text-align: left;padding: 0.4rem 1rem">Learning Goals</th>
            <th style="text-align: left;padding: 0.4rem 1rem">Environment</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align: left">Metasploit</td>
            <td style="text-align: left">Teach the basics of metasploit as well as basic reconnaissance and enumeration.</td>
            <td style="text-align: left">1 Linux host serving 3 vulnerable services.</td>
        </tr>
        <tr>
            <td style="text-align: left">Webapp</td>
            <td style="text-align: left">Teach the basics of website enumeration, tools, and overall methodology. Also include basic injection techniques.</td>
            <td style="text-align: left">1 Linux hosting an intentionally vulnerable website.</td>
        </tr>
        <tr>
            <td style="text-align: left">Active Directory</td>
            <td style="text-align: left">Teach common windows privesc as well as common AD attacks.</td>
            <td style="text-align: left">1 Windows domain controller with common AD misconfigurations.</td>
        </tr>
    </tbody>
</table>


I figured that Webapp week would most likely be the most fun for me to teach. It has some pretty interesting attacks you could play with and is, well hopefully, easy enough to teach to beginners as well.

## The Birth of JimCoin
---
![](https://i.imgur.com/IFhIZ10.png)

To get started, I needed a basic web application to work with. A fellow colleague of mine provided me with his Computer Science class' final project. The website was named RedemptionNFT which hosted a platform for people to buy, share, and list NFTs. The webapp was written in PHP which is convenient as PHP is known to be easily exploitable. So, I started developing the webapp.

The webapp had some nice basic functionality but I wanted more. Taking some pages out of HackTheBox, I wanted to implement some common website vulnerabilities that I dealt with. Vulnerabilities such as: 

<table style="margin-left:auto;margin-right:auto">
    <thead>
        <tr>
            <th style="text-align: left;padding: 0.4rem 1rem">Vulnerability</th>
            <th style="text-align: left;padding: 0.4rem 1rem">Summary</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align: left">SQL Injection</td>
            <td style="text-align: left">An injection technique based on improper sanitization of user input used to craft malicious SQL queries.</td>
        </tr>
        <tr>
            <td style="text-align: left">Local file inclusion</td>
            <td style="text-align: left">Unsafe file handling leading to exposure of local files.</td>
        </tr>
        <tr>
            <td style="text-align: left">Improper access controls</td>
            <td style="text-align: left">You aren't supposed to be here.</td>
        </tr>
    </tbody>
</table>

On top of these, I wanted to also teach the basic methodology of attacking a webapp so I included materials going over the usage of Burpsuite, gobuster, and so on.

As the site was nearing it's completion, I noticed that the default currency that was being used, Etherium, was not going to cut it. We need something new, something that really expresses us as a group. Enter, JimCoin. JimCoin is just a fake currency made up by us as a parody of one of our members. The original owner of the webapp created a subdomain for the site called "Jimmy Flips" where you can click a button to flip a picture of our friend to gain JimCoin. Using these coins, you can now purchase NFTs.

## Presentation of the Material
---
![](https://i.imgur.com/A6ztnZf.png)

### Week 1
Our first week covered Metasploit. The leader of our Kill Squad members on the CPTC team led this presentation. He covered topics such as how to scan for vulnerabilities using Metasploit and how we can use it to our advantage. To be honest, I didn't realize how powerful metasploit was until he put it on full display.

### Week 2
Alright so this is my week. This week we went over the basics of web application penetration testing and boy was that s journey. I prepared roughly 40 slides of pure information and I'm sure I put people to sleep. It was a very informational presentation but I tried my best to explain the joys of high level web application penetration testing.

### Week 3
This week we covered how to attack Active Directory led by our third member of the Kill Squad. It was around this time that involvement from SWIFT tended to cease which really hurt us. Between us 3, we had to effectively run a whole month worth of teaching material. We were pretty short staved to say the least. Creating slides, study guides, and presenting the material is a hassle. Regardless, I did my best to facilitate the teaching process each week.

## The SCARE
---
![](https://i.imgur.com/aBONux5.png)

The SCARE is the conclusive event wrapping up Hacktober in a fun filled event. Each week we released an environment for the students to test the attacks we went over during each week and now, they get to see them all over again. The SCARE consists of 3 machines with the goal of each student attempting a penetration test on the boxes. The goal is to submit as many findings in detailed support tickets to the support desk to gain points. You get 100 points per foothold/privesc and 50 points per web vulnerability. We had a decent turnout of people but boy, only having 2 people man the support desk is stressful.

Tickets began pouring in and piling up. I tried my absolute best to respond to everyone's ticket in a thoughtful manner, but with the overwhelming amount of tickets being submitted, that would prove to be impossible. On the bright side, it is nice to see so many students excelling at the event as they find a handful of vulnerabilities to report.

In the end, my mentee ended up winning the event by a large margin. The second place had roughly 2/3 the amount of points he did. But aside from winners and losers, the real takeaway of this event for everyone is knowledge. The people who stuck around from the beginning of the month to the end of the event, they all learned a significant deal about penetration testing.

## Conclusion
---
![](https://i.imgur.com/3NUzUCc.jpg)

Overall Hacktober was a success, but we can definitely use some improvements. First and foremost, I would've liked some more help from my peers as I did notice people taking a step back as the CPTC Kill Squad took over the month of teaching. We are only 3 people and we can only do so much. This put a lot of strain on us and phew was it stressful.

Aside from those issues, this month in particular, I really enjoyed teaching people. During the workshops, the workshop lead would be teaching materials and going along while the other 2 helpers would go around and make sure everyone is on the same page. As I went around explaining details, I clearly remember seeing light bulbs pop up inside the students head as the fog cleared for them. Being able to visibly see students connect the dots is a very nice moment for me. I really enjoy teaching these people who have similar interests that I do. If anyone is curious and would like to view some of the development process into the web application, I have my notes [here](https://succulent-lentil-32e.notion.site/Capstone-Project-4ad5827d988a432fa28cbfa6961adf63).

