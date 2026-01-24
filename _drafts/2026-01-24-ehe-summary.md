---
layout: post
title: Ethical Hacking Essentials in a Nutshell
date: 2026-01-17 00:00:00
description: My thoughts on EC-Council's EHE
tags: certification 
categories: ethical-hacking review
---


### Module 01: Information Security Fundamentals

This module discussed the following: PCI-DSS, ISO/IEC 27001, HIPAA, SOX, DMCA, FISMA, GDPR, DPA—in other words—**GRC**. Governance, Risk, and Compliance or GRC is an area of information security that ensures a company doesn't get sued. Any IT professional getting into cybersecurity should be familiar with these acronyms because they govern what can and what can't be done. After all, if you're into ethical hacking, you don't want to land in jail.

### Module 02: Ethical Hacking Fundamentals

Get inside the mind of a hacker. In this module, the cyber kill chain methodology was covered, along the types and components of malware, and an introduction to a few hacking tools. This is where the real hacking began.

The lab consisted of conducting passive footprinting using Web Data Extractor and Whois lookup; network scans using traceroute, Nmap, Unicornscan, and MegaPing; and NETBIOS enumeration using Nbtstat.

### Module 03: Information Security Threats and Vulnerability Assessment

A script kiddie's dream. This module elaborated on the importance of information security, but more so on how it can be compromised using off-the-shelf applications.

The lab instructed the use of GUI-based remote access tools and virus makers. The last room, however, redeemed this module from being too unethical by using Greenbone, an OpenVAS vulnerability scanner. With this tool, we were able to detect a TCP port vulnerable to RPC enumeration, which can be exploited for lateral movement (<a href="https://attack.mitre.org/tactics/TA0008/">MITRE ATT&CK TA0008</a>).

### Module 04: Password Cracking Techniques and Countermeasures

I love this module. I have fundamental understanding of algorithms used in password hashing, so I was excited to dive into cracking passwords.

Compared to Module 02, where we used passive tools, this module used active attack tools. Before we get "cracking", we need to capture hashes first. We used **Responder**, an LLMNR, NBT-NS, MDNS poisoner. This was used to listen to file share communications over the SMB protocol by spoofing as a legitimate name resolver. Once our victim machine tried to retrieve a file share, it connected to the "totally legitimate" name resolver. Thus, Responder was able to listen and capture NTLMv2-SSP hashes, along with its IP Address and username. Now that we have the hash, we cracked it using **John the Ripper** with a wordlist. After a few minutes, John was able to find a plaintext password matching the hash of the victim. Since we have the IP address, username, and password at hand, we can now connect to the file share as the victim. This is essentially an Adversary-in-the-Middle attack of the type LLMNR/NBT-NS Poisoning and SMB Relay (<a href="https://attack.mitre.org/techniques/T1557/001/">MITRE ATT&CK T1557.001</a>) to steal account names and passwords (<a href="https://attack.mitre.org/tactics/TA0006/">MITRE ATT&CK TA00006</a>)

Since there are multiple layers on this attack, we need to also secure those very layers. First, shut the doors at the transport layer (OSI Layer 4). UDP ports 5355 and 137 should be disabled if LLMNR and NBT-NS are not required. Next is to enforce the use of SMB version 3.0 or later by disabling v1/v2. If your organization requires LLMNR and NBT-NS, we need to secure the network layer (OSI Layer 3) to be able detect anomalous traffic on those ports by installing devices such as Network Intrusion Detection/Prevention Systems (NIDS/NIPS) like Snort along the network, or at the endpoints with Host-Based Intrusion Detection/Prevention Systems (HIDS/HIPS) like SolarWinds SEM. And finally at the application layer (OSI Layer 7), at the very least, we should audit passwords using John the Ripper (yes, the same one we used to crack it).

### Module 05: Social Engineering Techniques and Countermeasures

### Module 06: Network Level Attacks and Countermeasures

### Module 07: Web application Attacks and Countermeasures

### Module 08: Wireless Attacks and Countermeasures

### Module 09: Mobile Attacks and Countermeasures

### Module 10: IoT and OT Attacks and Countermeasures

### Module 11: Cloud Computing Threats and Countermeasures

### Module 12: Penetration Testing Fundamentals

### Capstone Project