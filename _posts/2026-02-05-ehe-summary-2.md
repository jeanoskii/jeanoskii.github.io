---
layout: post
title: Ethical Hacking Essentials in a Nutshell (Part 2)
date: 2026-01-24 00:00:00
description: My thoughts on EC-Council's EHE.
tags: certification 
categories: ethical-hacking review
thumbnail: assets/img/posts/2026-01-24/EHE-1.webp
---

This is the second part of the review.Click <a href="https://jeanoskii.github.io/blog/2026/ehe-summary/">here</a> to read the first part. Let's get started.

### Module 07: Web Application Attacks and Countermeasures

Introduction to web application penetration testing. Here, the modules discussed the web server and web application architecture and vulnerability stacks available on the market. <a href="https://owasp.org/Top10/2025/">OWASP Top 10</a> are reinforced, including injection attacks which is performed at the lab.

For the lab, we cracked the FTP credentials of a web server using my favorite tool, John the Ripper. We also used sqlmap to craft SQL injection commands, while using Burp Suite to intercept web traffic. Referencing OWASP Top 10:2025, we were able to demonstrate how A01 - Broken Access Control, A04 - Cryptographic Failures, and A05 - Injection attacks are done. 

For mitigation, there is a section in OWASP Top 10 on "How to prevent." For A01, all files must be denied access except for public-facing assets. This would minimize directory traversal attacks. In addition, proper session management must be implemented which should be invalidated on the server after logout. For A04, enforce strong passwords to prevent a dictionary attack on a stolen hash. As well, use SFTP instead of FTP. Lastly, for A05, always sanitize user input. Most developers don't do this right away, but should be refactored once the functionality passes unit tests. If you're not using an ORM, make your all SQL statements are either in a parameterized statement on the web app back end, or as a prepared statement or stored procedure on the DBMS.

While I was teaching web programming, I make it an advocacy to introduce security concepts to my students. Once they are familiar with writing CRUD, I would introduce them to Object-Relational Mapping (ORM). For Java, I would point them to Hibernate, whereas for ASP.NET I'd tell them to use Entity Framework. I'd often pique their curiosity towards industry tech stacks, by comparing their efforts on handwriting SQL commands, versus how quick and effortless it would have been with an ORM.

**8.5 out of 10—exploring all of the OWASP Top 10 would make this a 10.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-01-24/harvey-specter-suits.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 08: Wireless Attacks and Countermeasures

Finally, the answer to everybody's question: Can you hack my neighbor's wifi? It depends.

This section discussed the wireless concepts such as 802.11, WEP and WPA, wireless packet capturing, with the addition of Bluetooth. All well and good, however I find that labs lacking the "hands-on" approach and I understand why.

Cracking wifi requires special hardware. For starters, you need to have a wireless NIC that supports monitor mode and packet injection. Not all wireless adapters have this, you need specific wireless cards. Aircrack-ng has a list of <a href="https://aircrack-ng.org/doku.php?id=compatibility_drivers">compatible cards</a>.

Therefore, the labs cannot replicate actual wireless packet capture with actual hardware. So students can't roleplay as a hacker at coffee shop. What we had for the labs was trusty Wireshark, and wireless capture file. We used Wireshark to analyze the packet capture, and perform wifi password cracking with John the Ripper. We used a dictionary attack on both WEP and WPA packet captures.

Password cracking mitigation always starts with not using bad passwords. Dictionary attacks are successful because bad passwords exists. Use strong passwords, everyone. Oh, and use WPA2...WPA3 if supported.

**7 out of 10—not too bad.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-01-24/harvey-specter-suits.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 09: Mobile Attacks and Countermeasures

I was expecting some kind of low-level malware injection on popular APK files and stealthy sideloading techniques.

This module talks about the anatomy of mobile attacks and various ways to spy on smartphones. I wasn't aware before how large and lucrative Android cyberattacks are. Ever since, I practiced responsible, privacy-respecting phone usage, so I only download from official sources. However, most attacks happen because users download and install infected apps from unofficial app stores. The labs emphasized this.

The Metasploit framework was used, specifically Msfvenom, to create a payload for a target Android VM. We pretended to be a dumb user by not reading the Android security prompts on installation, and just clicked "Yes"/"Install" to everything. And just like that, we have established a C2 communication to the device.

So how to mitigate? Only. Use. Official. App. Stores.

**8 out of 10—could've been followed up with APK disassembly and analysis.**

### Module 10: IoT and OT Attacks and Countermeasures

This is honestly an area I have no prior formal knowledge in at all. So it was a nice change of pace; I absorbed all the learning materials for this module.

Internet of Things and Operational Technology are the main focus of this section. This discusses IoT and OT concepts, such as terminologies, technologies, and protocols used. I was particularly interested in OT, probably because how it affects critical infrastructure and how devastating attacks could be. Remember Stuxnet?

For the labs, we used online footprinting tools such as Shodan and Google Hacking to discover vulnerable IoT and OT interfaces. IoT packet analysis was also done with Wireshark, where we captured and analyzed MQTT traffic.

We can't really prevent footprinting because it is mostly a passive activity. However, we can minimize the online-facing services and secure those which inevitable requires online presence. As for MQTT, it is inherently not a secure protocol. So avoid sending sensitive information, and you should be okay.

**9 out of 10—because OT attacks gets you on Watch Dogs level of black hat hacking.**

### Module 11: Cloud Computing Threats and Countermeasures

### Module 12: Penetration Testing Fundamentals

### Capstone Project