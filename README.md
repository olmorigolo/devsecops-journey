# IT Training Resources

## Training Resources

I haven't tested all the labs, but when I do, I'll update this page with my review.

### Online learning platforms

#### Cybrary.it

To begin I got a[ www.cybrary.it ](https://www.cybrary.it)subscription which is not too expensive (about 300$ a year). Their staff is always online (on slack ) and available for questions. There is plenty of introductory content on different security topics. I currently do the Pentesting Career path and watch some introductions on other topics too (SOC analysis, network security, etc...).&#x20;

The DevSecOps Fundamentals are really good and lets you create your own pipeline on your local computer. The Docker Intro is very complete too and has detailed explanation of how docker works. Unfortunately docker security is left out in this video.

#### Pluralsight

[https://www.pluralsight.com/](https://www.pluralsight.com) covers different IT fields not only security. It is around 24$/month and highly recommended from friends. I don't have experience with it yet.

#### edx

Last but not least, another online learning platfom [https://www.edx.org/](https://www.edx.org) is well known because they partner up with popular universities such as harvard, berkeley, or the MIT. Search for the keyword edx on linkedin, you'll see it's wideley spread. The courses are based on a lot of text and some videos. The content quality really depends on the lecturer and publisher. There are different universities providing courses on this platform. Some are outdated. I did some Introductions to kubernetes from the linux foundation on this platform. It was a lot to read but included lot's of hands-on material that I could just copy on my own personal computer.

**EC-Council Code Red**

Another course library from one of the biggest IT Security certification company, EC-Council. I didn't try yet but the content looks promosing. More on [https://codered.eccouncil.org/](https://codered.eccouncil.org)

#### pentester academy

I am new to it, but this academy is awesome. They structured the labs into different attack vectors. Lab access is via browser. Access is around 250$/year. Visit [https://www.pentesteracademy.com/onlinelabs](https://www.pentesteracademy.com/onlinelabs)

You can earn badges for some of the labs played.They now offer bootcamps in a price range between 300-500$, time 4-5 weeks.&#x20;

They have good labs for testing container security. Browser-based.&#x20;

### Online labs for common security tools

In addition to the online classes, popular tools have their own online labs. BurbSuite for instance launched their web security academy [https://portswigger.net/web-security](https://portswigger.net/web-security) in order to learn WebAppSecurity hands-on with their tool named BurbSuite. The community edition is just fine for learning about web security. OWASP Zap published videos on [https://owasp-academy.teachable.com/p/owasp-zap-tutorial](https://owasp-academy.teachable.com/p/owasp-zap-tutorial).

#### Gamified learning of unix tools and hacking

Become fluent in Linux basic commands with the wargame "bandit" at [https://overthewire.org/wargames](https://overthewire.org/wargames). There are also different older hacking games to play. Don't forget to support the platform.

If you are stuck, find the write-up for each level here: [https://hackmethod.com/overthewire-bandit/](https://hackmethod.com/overthewire-bandit/) I stuck at level 24 and I compared my solution with the one from the write-up and they are the same.  It still doesn't work. So there is a chance that the game is broken.&#x20;

#### CTF's / Capture the Flags and online hacking events

Train yourself using one of the online hacking CTF platforms listed below in my personal training list. The most recommended one is [https://www.hackthebox.eu/](https://www.hackthebox.eu) that I personally use. Find a team on reddit and join their discord channel. Let you guide by the team members. You can play the CTF's together. Hackthebox also hosts live hacking events that you can join on their twitch channel [https://www.twitch.tv/hackthebox](https://www.twitch.tv/hackthebox)

I learn a lot by just watching the video streams of programming or hacking sessions of my team on discord.

A good list of upcoming CTF's is available on [https://ctftime.org/ctfs](https://ctftime.org/ctfs)

#### Github/Dockerhub/Gitlab

On github there are plenty curated lists of tools and learning projects. Once you've started learning you might consider get more practical. Check out these tools:

[https://github.com/hysnsec/DevSecOps-Studio](https://github.com/hysnsec/DevSecOps-Studio) a distro simulation a DevSecOps environment that you can use in a vm as a playground.

[https://github.com/archerysec/archerysec](https://github.com/archerysec/archerysec) a DevSecOps Vulnerability Management tool

### Social & Communities

I follow: bulls eye from hackingpassion.com, hackernoon.com, Carlos from [https://book.hacktricks.xyz/](https://book.hacktricks.xyz)

### Learning Videos

[https://www.hacker101.com/sessions/pentest\_owasp](https://www.hacker101.com/sessions/pentest\_owasp) A Starters Guide to Pentesting with OWASP

[https://www.youtube.com/watch?v=2\_lswM1S264](https://www.youtube.com/watch?v=2\_lswM1S264) Ethical Hacking 1x1 from freeCodeCamp. Old, but still useful.

### Your home environment/material

I would recommend that you test different pentesting OS. Install VirtualBox with kali, parrot and blackarch. You could also dual boot a pentesting distro on your laptop. I personally work directly with kali linux on an old robust thinkpad and have a seperate macbook pro for training videos, surfing or doing non-security related things.

###

## My personal list of training resources

From It Test Engineer to DevSecOps Engineer

{% embed url="https://docs.google.com/spreadsheets/d/1KUgUj8jjeWA41buoTyYKpzSN8fll8LL-F0Zjd9wnhek/edit?usp=sharing" %}



| Type          | Subject          | Name & Link                                                                                                                                                   | Time & Planning                                                                                                                                                                                                                                                                                                                                                                                                         | Hours | Price (in $) | Review                                                                                                                                                                                                                                                                                       |
| ------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| hands-on lab  | Web App Security | [BurbSuite Web Traffic Interception Acedemy](https://portswigger.net/web-security/dashboard)                                                                  | <p>About 200 free labs for using BurbSuite with different web app attack vectors.<br>Apprentice Labs: 47, Practitioner Labs: 123, Expert Labs: 27<br>I estimated large, 1h per Lab and some buffer</p>                                                                                                                                                                                                                  | 200   | **0**        | Really good for learning vulnerabilities. The hints to resolve the puzzles are very good. You can even walktrough each assignment just for learning purposes.                                                                                                                                |
|               |                  |                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                         |       |              |                                                                                                                                                                                                                                                                                              |
| training      | Web App Security | [Zap Videos](https://www.zaproxy.org/zap-deep-dive/)                                                                                                          | 7h Videos                                                                                                                                                                                                                                                                                                                                                                                                               | 7     | 0            | Very detailed. One would maybe skip the first three which only explains installation and user interface.                                                                                                                                                                                     |
| training      | Web App Security | [Conference: OWASP App Security Training](https://training.owasp.org)                                                                                         | 2 days remote conference - next: 25 -26 may                                                                                                                                                                                                                                                                                                                                                                             | 16    | 500          |                                                                                                                                                                                                                                                                                              |
| membership    |                  | [Cybrary Pro Membership](https://www.cybrary.it)                                                                                                              | <p>Large library with introductive content: videos &#x26; online labs (browser).<br>They have a slack chat and mentors are very active &#x26; supportive.<br>1 year membership<br>The career paths are very interesting because they cover a lot of subjects, so you get introduced to everything. You will need to get more training on the subjects though.<br>I've registered for the following classes/courses:</p> |       | 300          | Cybrary has a lot of network and official cyber security engineering content. Labs are good, but tests are sometimes covering the wrong content.                                                                                                                                             |
| training      | Pentesting       | -- Cybrary Training: Pentester Career Path                                                                                                                    | Covers networking, ethical hacking, many hacking tools, scanner, python for hackers, kali linux and some methodologies/theory, forensics                                                                                                                                                                                                                                                                                | 160   | 0            | The nmap course is really good and detailed. you learn really every command and how the different scan techniques differ from each other.                                                                                                                                                    |
| training      | DevOps           | - Cybrary Training: Intro to docker                                                                                                                           | For beginners docker, dockerfile, docker-compose and portainer                                                                                                                                                                                                                                                                                                                                                          | 5     | 0            | Unfortunately docker security is not covered but good overview of docker, dockerfile, docker-compose and portaine                                                                                                                                                                            |
| training      | DevSecOps        | -- Cybrary Training: DevSecOps Fundamentals                                                                                                                   | 5h training                                                                                                                                                                                                                                                                                                                                                                                                             | 5     | 0            | Highly recommend. Covers a complete DevSecOps pipeline from installation to configuration to monitoring.                                                                                                                                                                                     |
| training      | Security Basics  | -- Cybrary Training: Certified Information Systems Security Professional (CISSP)                                                                              | <p>Course | 9 Items<br>Total time: 18h 15m</p>                                                                                                                                                                                                                                                                                                                                                                          | 18    | 0            |                                                                                                                                                                                                                                                                                              |
| training      | Pentesting       | [-- Cybrary Training: Kali Linux fundamentals](https://www.cybrary.it/course/kali-linux-fundamentals/)                                                        | 2 hours 9 minutes                                                                                                                                                                                                                                                                                                                                                                                                       | 3     | 0            |                                                                                                                                                                                                                                                                                              |
| training      | DevSecOps        | -- Cybrary Training: Configure Azure Kubernetes Service (AKS) Security                                                                                        | 1 hours online                                                                                                                                                                                                                                                                                                                                                                                                          | 1     | 0            |                                                                                                                                                                                                                                                                                              |
| training      | DevSecOps        | -- Cybrary Training:Intro to Docker                                                                                                                           | 7hrs & demo                                                                                                                                                                                                                                                                                                                                                                                                             | 7     |              | Very good overview of docker, docker compose basic commands,  portainer                                                                                                                                                                                                                      |
| training      | DevOps           | [The Linux Foundation: Introduction to devops](https://training.linuxfoundation.org/training/introduction-to-devops-and-site-reliability-engineering-lfs162/) | 10-12 Hours of Course Material                                                                                                                                                                                                                                                                                                                                                                                          | 12    | 0            |                                                                                                                                                                                                                                                                                              |
| training      | DevOps           | The Linux Foundation: Introduction to Kubernetes                                                                                                              | <p>20-36 hours of Course Material<br>Exam: 12h<br>Length: 4-5 weeks, 2-3 hours/week</p>                                                                                                                                                                                                                                                                                                                                 | 36    | 0            |                                                                                                                                                                                                                                                                                              |
| training      | DevOps           | The Linux Foundation: Introduction to cloud infra                                                                                                             | <p>50 Hours of Course Material<br>14 Weeks of Free Access to Online Course</p>                                                                                                                                                                                                                                                                                                                                          | 50    | 0            |                                                                                                                                                                                                                                                                                              |
| training      | Security Basics  | [The Linux Foundation: Online Training Introduction to linux security](https://training.linuxfoundation.org/training/linux-security-fundamentals/)            | <p>50 Hours of Course Material<br>14 Weeks of Free Access to Online Course</p>                                                                                                                                                                                                                                                                                                                                          | 50    | 0            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Unix Basics Lab  | [https://overthewire.org/wargames/bandit](https://overthewire.org/wargames/bandit)                                                                            | <p>34 Levels. Find the password for the next level using unix commands<br>I estimated 10 hours to complete it if you need to research every single level and the associated commands</p>                                                                                                                                                                                                                                | 10    | 0            | I did not finish all of them, stucked at level 23 so far                                                                                                                                                                                                                                     |
| hands-on lab  | Hacking Lab      | [https://tryhackme.com/](https://tryhackme.com)                                                                                                               | Tryhackme is a platform with virtual machines that need to be solved through walkthroughs, which is very good for beginners and normal CTFs where you self must hack into the machines.                                                                                                                                                                                                                                 | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.root-me.org/](https://www.root-me.org)                                                                                                           | Rootme is another page for online hosted virtual machines to hack.                                                                                                                                                                                                                                                                                                                                                      | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.vulnhub.com/](https://www.vulnhub.com)                                                                                                           | Vulnhub has machines to download and then to hack                                                                                                                                                                                                                                                                                                                                                                       | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.hackthebox.eu/](https://www.hackthebox.eu)                                                                                                       | Hackthebox has online machines to hack, but there are very limited in the free version.                                                                                                                                                                                                                                                                                                                                 | ?     | 0            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://hack.me/](https://hack.me)                                                                                                                           | This site seems to be a community platform                                                                                                                                                                                                                                                                                                                                                                              | ?     | 0            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.hacker101.com/](https://www.hacker101.com)                                                                                                       | free site with videos and CTFs                                                                                                                                                                                                                                                                                                                                                                                          | ?     | 0            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://crackmes.one/](https://crackmes.one)                                                                                                                 | This site has a lot of binaries for forensic learning.                                                                                                                                                                                                                                                                                                                                                                  | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.hackthissite.org/missions/basic/](https://www.hackthissite.org/missions/basic/)                                                                  | ?                                                                                                                                                                                                                                                                                                                                                                                                                       | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://attackdefense.com/](https://attackdefense.com)                                                                                                       | ?                                                                                                                                                                                                                                                                                                                                                                                                                       | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| hands-on lab  | Hacking Lab      | [https://www.hackthissite.org/](https://www.hackthissite.org)                                                                                                 | ?                                                                                                                                                                                                                                                                                                                                                                                                                       | ?     | ?            |                                                                                                                                                                                                                                                                                              |
| Certification | DevSecOps        | [Certification: DevSecOps Professionnal](https://www.practical-devsecops.com/certified-devsecops-professional/)                                               | <p>20-36 hours of Course Material<br>Exam: 12h<br>1 month 1-3h/day</p>                                                                                                                                                                                                                                                                                                                                                  | 36    | 800          | Labs are awesome and not too hard. You instantly use what youv'e learned durigng the course. I prepared for it with the DevSecOps Fundamentals course and Docker Intro on cybrary.it Team & Support all the time. They'll schedule 2 zoom calls with you to support your learning objective. |
|               |                  |                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                         |       |              |                                                                                                                                                                                                                                                                                              |
| Certification | DevOps           | [Certified Kubernetes Administrator](https://www.cncf.io/certification/cka/)                                                                                  | <p>35 Hours of Course Material<br>12 Months of Access to Online Course</p>                                                                                                                                                                                                                                                                                                                                              | 35    | 300          |                                                                                                                                                                                                                                                                                              |
| Certification | DevSecOps        | [Kubernetes Security](https://www.cncf.io/certification/cks/)                                                                                                 | <p>26-30 Hours of Course Material<br>12 Months of Access to Online Course<br>Exam: 2h</p>                                                                                                                                                                                                                                                                                                                               | 30    | 300          |                                                                                                                                                                                                                                                                                              |
| Certification | Pentesting       | [PEN-200](https://www.offensive-security.com)                                                                                                                 | <p>17+ hours of video<br>30/60/90 days of lab access<br>One exam attempt<br>mim 1 month<br>1-3 months</p>                                                                                                                                                                                                                                                                                                               | 40    | 1000         |                                                                                                                                                                                                                                                                                              |
| Certification | Pentesting       | [WEB-300 Pentesting](https://www.offensive-security.com)                                                                                                      | <p>10-hour video series<br>PDF course guide (410+ pages)<br>Private labs<br>Active student forums<br>Access to virtual lab environment</p>                                                                                                                                                                                                                                                                              | ?     | 1300         |                                                                                                                                                                                                                                                                                              |
| Certification | Pentesting       | [Certified Ethical Hacking](https://www.eccouncil.org/programs/certified-ethical-hacker-ceh/)                                                                 | <p>One Year Access to the CEH E-courseware<br>Six months access to EC-Council's official Online lab environment (i-Labs) 40 hours, you get 12 months access to the official EC-Council e-courseware, 12 months access to the pre-recorded videos, 6 months access to the ilabs and also the exam voucher is included in the package.<br>1h/day, finished in 3 months</p>                                                | 90    | 1813         |                                                                                                                                                                                                                                                                                              |
