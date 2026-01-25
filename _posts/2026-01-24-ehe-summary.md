---
layout: post
title: Ethical Hacking Essentials in a Nutshell (Part 1)
date: 2026-01-24 00:00:00
description: My thoughts on EC-Council's EHE.
tags: certification 
categories: ethical-hacking review
---

Ahhhh, EC-Council: providers of the (<a href="https://www.reddit.com/r/cybersecurity/comments/1m4lkhr/is_ceh_with_practical_worth_it_for_someone_with/">in</a>)famous Certified Ethical Hacker (CEH). Today, we are going to explore their other offerings. Ethical Hacking Essentials, or EHE, is an introductory course aimed at beginner red team professionals. For those who want to break stuff and write a report about how you broke it and how to fix it, this is for you!

There are a total of 12 modules in the course, and a final capstone project to test your skills. I'll be reviewing each module: what I learned, what I did, and what I realized. I will also be rating them on a 10-point scale, the meaning of which is 10 I enjoyed, and 1 I slept on. I will also be putting a picture of roughly my reactions or feelings to each module.

I will break this review into two parts. Let's start!

### Module 01: Information Security Fundamentals

This module discussed the following: PCI-DSS, ISO/IEC 27001, HIPAA, SOX, DMCA, FISMA, GDPR, DPA—in other words—**GRC**. Governance, Risk, and Compliance is an area of information security that ensures a company doesn't get sued. Any IT professional getting into cybersecurity should be familiar with these acronyms because they govern what can and what can't be done. After all, if you're going into ethical hacking, you don't want to land in jail.

**6 out of 10—because no one wants to go to jail.**

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/2026-01-24/harvey-specter-suits.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Module 02: Ethical Hacking Fundamentals

Get inside the mind of a hacker. In this module, the cyber kill chain methodology was covered, along the types and components of malware, and an introduction to a few hacking tools. This is where the real hacking began.

The lab consisted of conducting passive footprinting using Web Data Extractor and Whois lookup; network scans using traceroute, Nmap, Unicornscan, and MegaPing; and NETBIOS enumeration using Nbtstat.

**5 out of 10—too shallow for me, but it's a starter.**

### Module 03: Information Security Threats and Vulnerability Assessment

A script kiddie's dream. This module elaborated on the importance of information security, but more so on how it can be compromised using off-the-shelf applications.

The lab instructed the use of GUI-based remote access tools and virus makers. The last room, however, redeemed this lab from being too unethical by using **Greenbone**, an OpenVAS vulnerability scanner. With this tool, we were able to detect a TCP port vulnerable to RPC enumeration, which can be exploited for lateral movement (<a href="https://attack.mitre.org/tactics/TA0008/">MITRE ATT&CK TA0008</a>).

**5.5 out of 10—just for Greenbone.**

### Module 04: Password Cracking Techniques and Countermeasures

I love this module. I have fundamental understanding of algorithms used in password hashing, so I was excited to dive into cracking passwords.

Compared to Module 02, where we used passive tools, this module used active attack tools. Before we get "cracking", we need to capture hashes first. We used **Responder**, an LLMNR, NBT-NS, MDNS poisoner. This was used to listen to file share communications over the SMB protocol by spoofing as a legitimate name resolver. Once our victim machine tried to retrieve a file share, it connected to the "totally legitimate" name resolver. Thus, Responder was able to listen and capture NTLMv2-SSP hashes, along with its IP Address and username. Now that we have the hash, we cracked it using **John the Ripper** with a wordlist. After a few minutes, John was able to find a plaintext password matching the hash of the victim. Since we have the IP address, username, and password at hand, we can now connect to the file share as the victim. This is essentially an Adversary-in-the-Middle attack of the type LLMNR/NBT-NS Poisoning and SMB Relay (<a href="https://attack.mitre.org/techniques/T1557/001/">MITRE ATT&CK T1557.001</a>) to steal account names and passwords (<a href="https://attack.mitre.org/tactics/TA0006/">MITRE ATT&CK TA00006</a>)

Because there are multiple layers on this attack, we need to also secure those very layers. First, shut the doors at the transport layer (OSI Layer 4). UDP ports 5355 and 137 should be disabled if LLMNR and NBT-NS are not required. Next is to enforce the use of SMB version 3.0 or later by disabling v1/v2. If your organization requires LLMNR and NBT-NS, we need to secure the network layer (OSI Layer 3) to be able detect anomalous traffic on those ports by installing devices such as Network Intrusion Detection/Prevention Systems (NIDS/NIPS) like Snort along the network, or at the endpoints with Host-Based Intrusion Detection/Prevention Systems (HIDS/HIPS) like SolarWinds SEM. And finally at the application layer (OSI Layer 7), at the very least, we should audit passwords using John the Ripper (yes, the same one we used to crack it).

**10 out of 10—I mean, it's password cracking.**

### Module 05: Social Engineering Techniques and Countermeasures

The course content for this is excellent, but I found the lab lacking. Social engineering requires target research on the part of the attacker, this I am very much aware of. My problem? There was no target. Not even an imaginary victim. But nonetheless, the lab used open-source intelligence (OSINT) tools like **Google Hacking/Dorking** as well as **Social-Engineer Toolkit (SET)** to clone a website and craft a phishing email—which I sent to myself.

Email phishing could be minimized with security awareness training and regular audits. We can also filter emails at the application level, or at the network level with a firewall. Lastly, be aware of TMI: Too. Much. Information. OSINT succeeds when the target overshares anything and everything about themselves.

**2 out of 10—cyber hygiene: it's everyone's responsibility.**

### Module 06: Network Level Attacks and Countermeasures

This was hard for me. I had to review my computer networking concepts for this one. The content of this module talks about packet sniffing techniques, session hijacking and denial-of-service attacks.

The lab was equally as difficult. One room tasked us to compromise a network switch. We used MAC Flooding via **Macof** where fake MAC addresses are sent to the network switch with the goal of overloading the switch's MAC address tables (stored in content-addressable-memory). Once the table is full, the switch will now operate as a hub, where traffic is broadcasted to all nodes. Once we were able to receive traffic, we launched an ARP Poisoning attack via **Arpspoof** on the network in order to associate our own MAC address with the target IP address. By broadcasting numerous <a href="https://wiki.wireshark.org/Gratuitous_ARP">gratuitous ARP</a> replies/responses (remember, the switch is now a hub), the computers on the network will now associate the target IP address with the attacker's MAC address. In other words, the attacker is like a school bully shouting "I am 10.10.1.9"...on a megaphone...10 times per second...for 10 minutes. Eventually, everyone will believe the attacker is 10.10.1.9, even though the real 10.10.1.9 is the quiet kid on the corner.

Since every computer now knows that victim's IP is associated with the attacker's MAC address, every piece of traffic will now go to the attacker. Thus, we are able to intercept and modify the traffic as we please, before sending them to the target—and they could be none the wiser. We followed with a Session Hijacking attack. **OWASP ZAP (Zed Attack Proxy)** was used to act as a proxy server. Here, we captured session cookies and tokens transmitted insecurely via HTTP. We replayed the session data to the actual server, thus impersonating the victim. When that's not enough, we also phished the victim's username and password. With ZAP, we modified the capture HTTP packets to redirect the victim to a malicious website.

The attacks above are in an "assumed breach" scenario, where the attacker is already in the network. MAC flooding and ARP Poisoning (<a href="https://attack.mitre.org/techniques/T1557/002/">MITRE ATT&CK T1557.002</a>) can be minimized by disabling ARP Cache updates on gratuitous ARP replies/responses. We can also strengthen the network filters on the switches with Dynamic ARP Inspection (DAI). Similar with Module 04 mitigations, NIDS/NIPS can be used. For Session Hijacking (<a href="https://attack.mitre.org/techniques/T1539/">MITRE ATT&CK T1539</a>), we should enforce multi-factor authentication so that session data cannot be replayed. We should also check to make sure all traffic is sent with HTTPS. If there are traffic still in HTTP, some user training is in order. Literally all websites nowadays are in HTTPS, and it doesn't take too much effort to secure TLS certificates for a web app (thank you, <a href="https://letsencrypt.org/">Let's Encrypt</a>).

The last room instructed us to launch a DDoS attack with **High Orbit Ion Cannon (HOIC)**. Not that hard, really, which is why this is a very popular attack technique by script kiddies. We protected the asset with Anti-DDoS Guardian. We can also use similar software on the host, or use network firewalls, or even cloud-based solutions to protect against DDoS attacks.

All attacks above were monitored using Wireshark. Therefore, we have an idea what each attack looks like, from MAC flooding, ARP poisoning, session hijacking, and denial-of-service.

**9 out of 10—could be a 10 without HOIC.**

So far, I am quite satisfied with the first six modules. Stay tuned for Part 2, where I continue with Modules 07 to 12, plus the capstone project.