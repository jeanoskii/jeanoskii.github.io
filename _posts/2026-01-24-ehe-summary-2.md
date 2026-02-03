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

**8.5 out of 10—the labs could have explored all of the OWASP Top 10.**

<div class="row mt-3 mb-4">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-01-24/harvey-specter-suits.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


### Module 08: Wireless Attacks and Countermeasures

### Module 09: Mobile Attacks and Countermeasures

### Module 10: IoT and OT Attacks and Countermeasures

### Module 11: Cloud Computing Threats and Countermeasures

### Module 12: Penetration Testing Fundamentals

### Capstone Project