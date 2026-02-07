---
layout: post
title: Ethical Hacking Essentials in a Nutshell (Part 2)
date: 2026-02-05 00:00:00
description: My thoughts on EC-Council's EHE.
tags: certification 
categories: ethical-hacking review
thumbnail: assets/img/posts/2026-01-24/EHE-1.webp
---

This is the second part of the review.Click <a href="https://jeanoskii.github.io/blog/2026/ehe-summary/">here</a> to read the first part. Let's get started.

### Module 07: Web Application Attacks and Countermeasures

Introduction to web application penetration testing. Here, the modules discussed the web server and web application architecture and vulnerability stacks available on the market. <a href="https://owasp.org/Top10/2025/">OWASP Top 10</a> are reinforced, including injection attacks which is performed at the lab.

For the lab, we cracked the FTP credentials of a web server using my favorite tool, **John the Ripper**. We also used **sqlmap** to craft SQL injection commands, while using **Burp Suite** to intercept web traffic. Referencing OWASP Top 10:2025, we were able to demonstrate how <a href="https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/">A01 - Broken Access Control</a>, <a href="https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/">A04 - Cryptographic Failures</a>, and <a href="https://owasp.org/Top10/2025/A05_2025-Injection/">A05 - Injection</a> attacks are done. 

For mitigation, there is a section in OWASP Top 10 on "How to prevent." For A01, all files must be denied access except for public-facing assets. This would minimize directory traversal attacks. In addition, proper session management must be implemented which should be invalidated on the server after logout. For A04, enforce strong passwords to prevent a dictionary attack on a stolen hash. As well, use SFTP instead of FTP. Lastly, for A05, always sanitize user input. Most developers don't do this right away, but should be refactored once the functionality passes unit tests. If you're not using an ORM, make your all SQL statements are either in a parameterized statement on the web app back end, or as a prepared statement or stored procedure on the DBMS.

While I was teaching web programming, I make it an advocacy to introduce security concepts to my students. Once they are familiar with writing CRUD, I would introduce them to Object-Relational Mapping (ORM). For Java, I would point them to <a href="https://hibernate.org/">Hibernate</a>, whereas for ASP.NET I'd tell them to use <a href="https://learn.microsoft.com/en-us/ef/">Entity Framework</a>. I'd often pique their curiosity towards industry tech stacks, by comparing their efforts on handwriting SQL commands, versus how quick and effortless it would have been with an ORM.

**10 out of 10—you'd feel like a hackerman after this.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/hackerman.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 08: Wireless Attacks and Countermeasures

Finally, the answer to everybody's question: Can you hack my neighbor's wifi? It depends.

This section discussed the wireless concepts such as 802.11, WEP and WPA, wireless packet capturing, with the addition of Bluetooth. All well and good, however I find that labs lacking the "hands-on" approach and I understand why.

Cracking wifi requires special hardware. For starters, you need to have a wireless NIC that supports *monitor mode* and *packet injection*. Not all wireless adapters have this, you need specific wireless cards. **Aircrack-ng** has a list of <a href="https://aircrack-ng.org/doku.php?id=compatibility_drivers">compatible cards</a>.

Therefore, the labs cannot replicate actual wireless packet capture with actual hardware. So students can't roleplay as a hacker at coffee shop. What we had for the labs was trusty **Wireshark**, and wireless capture file. We used Wireshark to analyze the packet capture, and perform wifi password cracking with **John the Ripper**. We used a dictionary attack on both WEP and WPA packet captures.

Password cracking mitigation always starts with not using bad passwords. Dictionary attacks are successful because bad passwords exists. Use strong passwords, everyone. Oh, and use WPA2...WPA3 if supported.

**7 out of 10—not too bad.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/password.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 09: Mobile Attacks and Countermeasures

I was expecting some kind of low-level malware injection on popular APK files and stealthy sideloading techniques.

This module talks about the anatomy of mobile attacks and various ways to spy on smartphones. I wasn't aware before how large and lucrative Android cyberattacks are. Ever since, I practiced responsible, privacy-respecting phone usage, so I only download from official sources. However, most attacks happen because users download and install infected apps from unofficial app stores. The labs emphasized this.

The **Metasploit** framework was used, specifically **Msfvenom**, to create a payload for a target Android VM. We pretended to be a dumb user by not reading the Android security prompts on installation, and just clicked "Yes"/"Install" to everything. And just like that, we have established a *C2 communication* to the device.

So how to mitigate? Only. Use. Official. App. Stores.

**8 out of 10—could've been followed up with APK disassembly and analysis.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/edp-i-mean-its-all-right.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 10: IoT and OT Attacks and Countermeasures

This is honestly an area I have no prior formal knowledge in at all. So it was a nice change of pace; I absorbed all the learning materials for this module.

Internet of Things and Operational Technology are the main focus of this section. This discusses IoT and OT concepts, such as terminologies, technologies, and protocols used. I was particularly interested in OT, probably because how it affects critical infrastructure and how devastating attacks could be. Remember *Stuxnet*?

For the labs, we used online footprinting tools such as **Shodan** and **Google Hacking** to discover vulnerable IoT and OT interfaces. IoT packet analysis was also done with **Wireshark**, where we captured and analyzed *MQTT* traffic.

We can't really prevent footprinting because it is mostly a passive activity. However, we can minimize the online-facing services and secure those which inevitable requires online presence. As for MQTT, it is inherently not a secure protocol. So avoid sending sensitive information, and you should be okay.

**9 out of 10—because OT attacks gets you on Watch Dogs level of black hat hacking.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/watchdogs.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 11: Cloud Computing Threats and Countermeasures

Historically, this has been an area where paying attention matters. Bills could pile up and you wounldn't have a clue. (ask me how I know). And if that's not bad enough, if you leave your API key out in the open, anybody on the Internet can use your cloud instances.

These disadvantages were reiterated in the modules. As well, cloud specific attack vectors were discussed. And since the idea of a cloud is a computer on the Internet, we attack and secure it just like any other computer. Unlike an on-prem computer, we also need to consider cloud-centric technologies. We all know almost everything is going on the cloud, therefore this module is very relevant.

We exploited Amazon S3 buckets by first enumerating them using **lazys3**...and that's about everything the lab has.

So to secure cloud: keep an eye on your usage; secure your API keys; review IAM and ACL policies; and regularly audit cloud services.

**3 out of 10—where's the lab!?**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/where-you.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 12: Penetration Testing Fundamentals

As the last module, here's where all the prior modules are tied together to give us an birdseye view of how each piece could be used in penetration testing. We discussed the need to conduct penetration testing, and its stages: Reconnaissance, Scanning, Vulnerability Assessment, Exploitation, and Reporting.

No labs here though.

**0 lab, 0 rating**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/bored.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Capstone

Probably the section most readers look forward to—what are my thoughts and experience in the capstone project? This is a capture-the-flag (CTF) style capstone, where the goal is to capture a specific string and paste it on the textbox. If a flag is valid, the system will add it to your bank of flags. If I recall correctly, there are 8 flags to be captured within 1 hour. These need not be captured sequentially, so skipping is allowed. To pass, I believe you need to capture at least 6 flags. On my first attempt, I managed to get only 3. Yeah, not a great start.

I didn't refer to my notes on my first attempt, as I wanted to test my current knowledge.  Luckily, there are unlimited attempts for the capstone. So on my second attempt, I used my notes. Now, I managed to get 6 flags with 20 minutes remaining. I wasn't able to get another when the time ran out.

I would like to stress the importance of taking down notes. In fact, this is what ethical hackers and pentesters do most of the time. Documenting every key detail is useful, and I was thankful I did this at the very beginning. With my notes, I was able to quickly jump through each attack strategy upon discovering a vulnerability. Also, following the stages of penetration testing is helpful. Whenever you get lost, you can always go back to recon.

As for the apps I used, I won't go into detail—I will just list them: Responder, John the Ripper, Arpspoof, Nmap, Shodan, WHOIS lookup, OWASP ZAP, Web Data Extractor, sqlmap, and Wireshark.

**10 out of 10—perfect difficulty.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/penguinz.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


### Conclusion

Overall, for anyone who would like to get their toes wet in the world of ethical hacking, EC-Council's Ethical Hacking Essentials (EHE) is a good start. It's also a great jump off point to the <a href="https://www.eccouncil.org/train-certify/certified-ethical-hacker-ceh/">Certified Ethical Hacker (CEH)</a> certification, since the structure of the modules, labs, and exam would likely be similar. To get certified, you will only need to pass the multiple-choice exam. You don't even need to finish the capstone.

However, there are also a multitude of similarly priced certifications targeting the same audience. For example, there's INE's <a href="https://ine.com/security/certifications/ejpt-certification">eLearnSecurity Junior Penetration Tester (eJPT)</a>, TCM's <a href="https://certifications.tcm-sec.com/pjpt/">Practical Junior Penetration Tester (PJPT)</a>, and THM's <a href="https://tryhackme.com/certification/junior-penetration-tester">Junior Penetration Tester (PT1)</a>. With these, you need to pass the practical exam similar to the capstone, before getting certified.

**EHE is a solid 7 out of 10—it is good on its own.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-02-05/brilliant-good.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

And with that, we've reached the end of this review. Keep hacking!