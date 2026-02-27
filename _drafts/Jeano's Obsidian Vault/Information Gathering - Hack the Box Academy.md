 - Introduction
	 - Information Gathering is collecting information about website or web application before conducting deeper analysis and potential exploitation.
	![[Pasted image 20260224161847.png]]
	- Goals of web reconnaissance:
		- Identifying Assets
			- Web pages, subdomains, IP addresses, and technologies used
		- Discovering Hidden Information
			- Backup files, configuration files, or internal documentation
		- Analyzing the Attack Surface
			- Identify potential vulnerabilities by assessing technologies used, configurations, and possible entry points
		- Gathering Intelligence
			- Identifying key personnel, email addresses, or patterns of behavior that could be exploited
	- Types of reconnaissance:
		- Active
			- Directly interacting with the target system, and has high risk of detection that could lead to being blacklisted
			- Forms of active reconnaissance:
				- Port Scanning
					- Identifying open ports and services running on the target using Nmap, Unicornscan
					- High risk of detection
				- Vulnerability Scanning
					- Probing for known vulnerabilities using scanners like Nessus, OpenVAS, Nikto
					- High risk of detection
				- Network Mapping
					- Identifying network topology and connected devices using Traceroute, Nmap
					- Medium to High risk of detection
				- Banner Grabbing
					- Retrieving information from banners displayed by services running on the target using netcat, curl
					- Low risk of detection
				- OS Fingerprinting
					- Identifying OS on the target using Nmap -o flag
					- Low risk of detection
				- Service Enumeration
					- Determining specific versions of services running on open ports using Nmap -sV flag
					- Low risk of detection
				- Web Spidering
					- Crawling target website to identify web pages, directories, and files using Burp Suite Spider, OWASP ZAP Spider
					- Low risk of detection
		- Passive
			- Does not interact with the target system directly and relies instead on publicly available information
			- Forms of passive reconnaissance:
				- Search Engine Queries
					- Google Hacking/Dorking and Shodan
				- WHOIS Lookups
					- Querying WHOIS databases to retrieve domain registration details 
				- DNS
					- Analyzing DNS records to identify subdomains, mail servers using `dig`, `nslookup`, `dnsenum`
				- Web Archive Analysis
					- Examining historical snapshots of website using Internet Archive/Wayback Machine
				- Social Media Analysis
					- Gathering employee and company information using LinkedIn, Facebook, Twitter, and specialized OSINT tools
				- Code Repositories
					- Searching through public repositories for exposed credentials or vulnerabilities using Github, GitLab
- WHOIS
	- Each WHOIS record contains the following:
		- Domain Name
			- e.g. example.com
		- Registrar
			- Company where domain is registered (GoDaddy, Namecheap)
		- Registrant Contact
			- Person or organization that registered the domain
		- Administrative Contact
			- Person responsible form managing the domain
		- Technical Contact
			- Person handling technical issues related to the domain
		- Creation and Expiration Dates
			- When the domain was registered and when it's set to expire (if the admins are slow to renew, it could lead to **cybersquatting**)
		- Name Servers
			- Servers that translate domain name to IP address
	- Originally invented in the 1970s by Elizabeth Feinler at Stanford Research Institute, to store information about network users, hostnames, and domain names on the ARPANET.
	- Formalized as a standard in 1982 with RFC 812
	- Vint Cerf, hailed as one of the "fathers of the internet" created Internet Corporation for Assigned Names and Numbers (ICANN) to assume responsibility for global DNS management and WHOIS policy development
	- WHOIS records reveal names, email address, and phone numbers of people responsible for managing the domain, which can be leverage in social engineering attacks (impersonation).
	- WHOIS records also reveal technical details of the name servers and IP addresses which can be used as clues about target's network infrastructure.
	- Historical WHOIS records can be viewed using [WhoisFreaks](https://whoisfreaks.com/) where ownership changes, contact information, or technical details change over time.
	- Utilizing WHOIS (Lab)
		- Scenarios:
			- Scenario 1: Phishing Investigation
				- Suspicious emails have been sent claiming from company's bank. A security analyst performed WHOIS lookup on the domain linked to the email and found:
					- Registration Date
						- Recently registered, just a few days ago
					- Registrant information
						- Hidden behind privacy service
					- Name Servers
						- Associated with a known bulletproof hosting provider often used for malicious activities
				- Findings are red flags that suggest phishing campaign. Analyst should alert IT department and remediate by blocking the associated domain, and inform employees.
			- Scenario 2: Malware Analysis
				- Malware communicates with a remote server using a domain. A researcher performed WHOIS lookup of the domain associated with the command-and-control (C2) server and found:
					- Registrant
						- Used free email service known for anonymity
					- Location
						- Registrant's address is in a country with high prevalence of cybercrime
					- Registrar
						- Domain was registered through a registrar with history of lax abuse policies
				- Conclusion is made the C2 is likely hosted on a compromised or "bulletproof" server.
				- Such is the case for **Wannacry**, where ransomware checks if a domain is registered. If not, the malware's payload is activated. If yes, it is deactivated.
			- Scenario 3: Threat Intelligence Report
				- Threat actor group uses multiple domains throughout their malicious campaigns. Analyst gather WHOIS data on these domains and found:
					- Registration Dates
						- Registered in clusters shortly before major attacks
					- Registrants
						- Uses various aliases and fake identities
					- Name Servers
						- Multiple domains share same name servers, suggesting common infrastructure
					- Takedown History
						- Many domains are taken down after attacks, indicating law enforcement intervention
				- Insights can create a detailed profile of the threat actor's tactics, techniques, and procedures (TTPs), and includes the WHOIS data collected as Indicators of Compromise (IOCs).
				- These reports can be used by other organizations to detect and block future attacks, or simulate the threat actor group in a red team activity to test security controls and policies.
		- Questions:
			1. Perform a WHOIS lookup against the paypal.com domain. What is the registrar Internet Assigned Numbers Authority (IANA) ID number?
				1. Run `whois paypal.com` or use https://whois.domaintools.com/
					![[Pasted image 20260224170406.png]]
				2. Answer is 292
			2. What is the admin email contact for the tesla.com domain (also in-scope for the Tesla bug bounty program)?
				1. Run `whois tesla.com` or use https://whois.domaintools.com/
					![[Pasted image 20260224170647.png]]
				2. Answer is admin@dnstinations.com
- DNS and Subdomains
	- How DNS works:
		1. Your Computer Asks for Directions (DNS Query)
			- If computer has the IP address cached in memory, it returns it immediately. Otherwise, proceed to step 2.
		2. DNS Resolver Checks Its Map (Recursive Lookup)
			- DNS Resolver, whether its ISP or public resolver like 1.1.1.1, will check if the IP address is cached in memory. Otherwise, proceed to step 3.
		3. Root Name Server Points the Way
			- Top-level servers in DNS hierarchy
		4. TLD Name Server Narrows It Down
			- Responsible for specific top-level domains (e.g. .com, .org)
		5. Authoritative Name Server Delivers the Address
			- Holds the actual IP address of the requested resource and returns it to the DNS resolver.
		6. DNS Resolver Returns the Information
			- Before sending back to client, DNS Resolver will save it to cache.
		7. Your Computer Connects
	- Hosts file is used to map hostnames to IP address.
		- For Windows, it is found in `C:\Windows\System32\drivers\etc\hosts`
		- TXT (Text)
			- Stores arbitrary text information, often used for domain verification or security policies
			- `example.com.` IN TXT `"v=spf1 mx -all"` (SPF record)
		- SOA (Start of Authority)
			- Specifies administrative information about a DNS zone
			- `example.com.` IN SOA `ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400`
		- SRV (Service)
			- Defines the hostname and port number for specific services
			- `_sip._udp.example.com.` IN SRV 10 5 5060 `sipserver.example.com.`
		- PTR (Pointer)
			- Used for reverse DNS lookups mapping IP address to a hostname
			- `1.2.0.192.in-addr.arpa.` IN PTR `www.example.com.`
	- DNS Tools:
		- `dig` (Domain Information Groper)
			- If you want only the answer, use `+short` command
		- `nslookup`
		- `host`
		- `dnsenum`
		- `fierce`
		- `dnsrecon`
		- `theHarvester`
		- Online DNS Lookup Services
	- Some servers can detect and block excessing DNS queries
	- DNS Lab
		- Questions:
			1. Which IP address maps to inlanefreight.com?
				1. Run command `dig +short inlanefreight.com`
					![[Pasted image 20260224204132.png]]
				2. Answer is 134.209.24.248
			2. Which domain is returned when querying the PTR record for 134.209.24.248?
				1. Perform reverse lookup using command `dig +short -x 134.209.24.248`
					![[Pasted image 20260224204943.png]]
				2. Answer is inlanefreight.com
			3. What is the full domain returned when you query the mail records for facebook.com?
				1. Run command `dig +short MX facebook.com`
					![[Pasted image 20260224205048.png]]
				2. Answer is smtpin.vvv.facebook.com
	- Subdomains are extensions of the main domain, often created to organize and separate sections or functionalities of a website. 
	- Common Subdomains:
		- blog.example.com
		- shop.example.com
		- mail.example.com
		- dev.example.com
		- staging.example.com
	- Subdomains may hold hidden login portals (not meant to be publicly accessible), legacy applications, or sensitive information/files.
	- Types of Subdomain Enumeration:
		- Active
			- Zone transfer
				- Copy zone file from a name server to the attacker's DNS to discover subdomains
			- Brute-force enumeration
				- Test a list of potential subdomain list against target domain using tools like `dnsenum`, `ffuf`, and `gobuster` paired with wordlists like `fierce-hostlist
		- Passive
			- Certificate Transparency (CT) Logs
				- Public repositories of SSL/TLS certificates, which often include list of associated subdomains in their Subject Alternative Name (SAN) field
			- Search Engines
				- Use of search operators such as `site:` to show subdomain results
	- Subdomain Brute-forcing Lab
		- Use `dnsenum` together with `subdomains-top1million-5000.txt` wordlist
			- `dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r`
		- Questions:
			1. Using the known subdomains for inlanefreight.com (www, ns1, ns2, ns3, blog, support, customer), find any missing subdomains by brute-forcing possible domain names. Provide your answer with the complete subdomain, e.g., www.inlanefreight.com.
				1. Use `fierce-hostlist` to enumerate: `dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/fierce-hostlist.txt`
					![[Pasted image 20260224211126.png]]
				2. Answer is my.inlanefreight.com
	- DNS Zone Transfer Lab
		- Less invasive and more efficient than brute-forcing
		- To remediate this, only allow zone transfer from trusted secondary servers
		- Use `dig` with the `axfr` command
			- `dig axfr @nsztm1.digi.ninja zonetransfer.me`
		- Questions:
			1. After performing a zone transfer for the domain inlanefreight.htb on the target system, how many DNS records are retrieved from the target system's name server? Provide your answer as an integer, e.g, 123.
				1. Perform zone transfer from name server 10.129.42.195 for the domain inlanefreight.htb: `dig axfr @10.129.42.195 inlanefreight.htb`
					![[Pasted image 20260224212420.png]]
				2. Answer is 22
			2. Within the zone record transferred above, find the ip address for ftp.admin.inlanefreight.htb. Respond only with the IP address, eg 127.0.0.1
				1. Look upwards on the retrieved records for `ftp.admin.inlanefreight.htb`
					![[Pasted image 20260224212535.png]]
				2. Answer is 10.10.34.2
			3. Within the same zone record, identify the largest IP address allocated within the 10.10.200 IP range. Respond with the full IP address, eg 10.10.200.1
				1. Look for records with IP starting in 10.10.200 and identify the IP address with the largest last octet
					![[Pasted image 20260224212810.png]]
				2. Answer is 10.10.200.14
	- Virtual Hosts Lab
		- Web servers like Nginx, Apache, IIS, can host multiple websites with the same IP address using virtual hosting, taking advantage of `HTTP Host` header
		![[Pasted image 20260224213325.png]]
		- Subdomains have their own DNS records, whereas Vhosts do not.
		- Websites often have subdomains that are not public and won't appear in DNS records even after zone transfer.
		- Vhost fuzzing is a technique to discover public and non-public subdomains and vhosts by testing various hostnames against a known IP address
		- Types of Virtual Hosting::
			- Name-based
				- Relies solely on `HTTP Host` header to distinguish between websites.
				- Most common, cost-effective, easy to set up
				- But requires a web server to support name-based virtual hosting and can have limitations with certiain protocols like SSL/TLS
			- IP-based
				- Assigns unique IP address to each website, and the server determines which website to serve based on which IP address the request was sent.
				- Doesn't rely on `HTTP Host` header and can be used with any protocol, offers better isolation between websites
				- Can be expensive and less scalable, as it required multiple IP addresses
			- Port-based
				- Websites are assigned a specific port to which website will be served
				- Useful when IP addresses are limited
				- But not as common or user-friendly as name-based virtual hosting and may require users to specify port number in URL.
		- Virtual Host Discovery Tools
			- `gobuster`
				- Often used for directory/file brute-forcing, and can be used for vhost discovery.
				- Fast, supports multiple HTTP methods, can use custom wordlists
				- Must prepare the following first:
					- Target Identification (through DNS lookup or other recon techniques)
					- Wordlist Preparation (use pre-compiled wordlist such as SecLists or create custom one)
				- `gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain`
					- `-u` flag specifies target URL/IP address
					- `-w` specifies wordlist; provide path
					- `--append-domain` appends base domain to each word in the wordlist (brute-forcing)
					- `-t` to set number of threads for performance
					- `-k` ignore SSL/TLS certification errors
					- `-o` to save output to a file
			- `feroxbuster`
				- Rust-based implementation of `gobuster` known for speed and flexibility
				- Supports recursion, wildcard discovery, and various filters
			- `ffuf`
				- Web fuzzer that can be used for vhost discovery by fuzzing the `Host` header
				- Customizable wordlist input and filtering options
		- Vhost brute-forcing can generate significant traffic and can be detected by intrusion detection systems (IDS) or web application firewalls (WAF).
		- Questions:
			1. Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "web"? Answer using the full domain, e.g. "x.inlanefreight.htb"
				1. Modify local hosts files using `sudo nano /etc/hosts`
				2. Add entry for `154.57.164.75    inlanefreight.htb`
				3. Run command `gobuster vhost -u http://inlanefreight.htb:31082 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 40` (`-t` to use 40 threads to make it faster)
				4. Look for entry prefixed with `web`
				5. Answer is web17611.inlanefreight.htb
			2. Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "vm"? Answer using the full domain, e.g. "x.inlanefreight.htb"
				1. Look for entry prefixed with `vm`
				2. Answer is vm5.inlanefreight.htb
			3. Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "br"? Answer using the full domain, e.g. "x.inlanefreight.htb"
				1. Look for entry prefixed with `br`
				2. Answer is browse.inlanefreight.htb
			4. Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "a"? Answer using the full domain, e.g. "x.inlanefreight.htb"
				1. Look for entry prefixed with `a`
				2. Answer is admin.inlanefreight.htb
			5. Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "su"? Answer using the full domain, e.g. "x.inlanefreight.htb"
				1. Look for entry prefixed with `su`
				2. Answer is support.inlanefreight.htb
	- Certificate Transparency Logs
		- Attackers can exploit rogue or mis-issued certificates to impersonate legitimate websites, intercept sensitive data, or spread malware.
		- Certificate Transparency (CT) Logs are public, append-only ledgers that record issuance of SSL/TLS certificates.
		- Whenever Certificate Authority (CA) issues new certificate, it must submit it to multiple CT logs
		- Purpose of CT Logs:
			- Early detection of rogue certificates
				- A rogue certificate is an unauthorized or fraudulent digital certificate issued by a trusted CA.
			- Accountability for Certificate Authorities
				- CA's are accountable for their issuance practices and will be publicly visible on the CT log.
			- Strengthening the Web PKI
		- How CT Logs work:
			1. Certificate Issuance
				- Website owner requests SSL/TLS certificate from CA and issues pre-certificate
			2. Log Submission
				- CA submits pre-certificate to multiple CT logs
			3. Signed Certificate Timestamp (SCT)
				- Upon receiving pre-certificate, each CT log generates SCT, which is a cryptographic proof that the certificate was submitted to the log at a specific time.
			4. Browser Verification
				- Web browser will check certificate's SCT against public CT logs. Once validated, browser establishes secure connection. If not, will warn the user.
			5. Monitoring and Auditing
				- CT logs are monitored by browser vendors, website owners, and security researchers
		- CT logs employ Merkle tree cryptographic structure:
			![[Pasted image 20260225140829.png]]
		- CT Logs and Web Recon
			- CT logs offer historical records (including expired certificates) of the website, and might reveal information of (outdated) software and configurations
			- Tools for searching CT Logs:
				- [crt.sh](https://crt.sh/)
					- Offers both web-based GUI and API for CLI-based applications:
						- `$ curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[] | select(.name_value | contains("dev")) | .name_value' | sort -u`
				- [Censys](https://search.censys.io/)
- Fingerprinting
	- Focuses on extracting technical details about the technologies powering a website.
	- Purpose of Fingerprinting:
		- Targeted Attacks
		- Identifying Misconfigurations
		- Prioritizing Targets
		- Building a Comprehensive Profile
	- Fingerprinting Techniques:
		- Banner Grabbing
			- Banners reveal server software, version numbers, and other details
		- Analyzing HTTP Headers
			- `Server` header reveal web server software, while `X-Powered-By` header reveal scripting languages or frameworks used.
		- Probing for Specific Responses
			- Certain error messages or behaviors could reveal web server or software components
		- Analyzing Page Content
			- Web page structure, copyright header, scripts, and other elements can reveal underlying technologies
	- Fingerprinting Tools:
		- Wappalyzer
			- Browser extension and online service for web technology profiling
		- BuiltWith
			- Web technology profiler
		- WhatWeb
			- Command-line tool for website fingerprinting
		- Nmap
			- Network scanner used for service and OS fingerprinting
		- Netcraft
			- Detailed report on website technology, hosting provider, security posture
		- wafw00f
			- Command-line tool for identifying presence and type of web application firewall (WAF)
	- Fingerprinting inlanefreight.com
		- Use `curl` with `-I` flag (or `--head`) to fetch only HTTP headers
			- `curl -I inlanefreight.com`
				- reveals the server version and location
			- `curl -I https://inlanefreight.com`
				- reveals `X-Redirected-By: WordPress`
			- `curl -I https://www.inlanefreight.com`
				![[Pasted image 20260225142858.png]]
				- reveals `wp-json` and `wp-` prefix related to Wordpress
		- Use `wafw00f`
			- Install using `pip3 install git+https://github.com/EnableSecurity/wafw00f`
			- Run command `wafw00f inlanefreight.com`
				![[Pasted image 20260225142832.png]]
				- Reveals it is protected by Defiant
		- Use Nikto
			- A powerful open-source web server scanner
			- Install with the following commands:
				1. `sudo apt update && sudo apt install -y perl`
				2. `git clone https://github.com/sullo/nikto`
				3. `cd nikto/program`
				4. `chmod +x ./nikto.pl`
			- Run command `nikto -h inlanefreight.com -Tuning b` where `-h` indicates host and `-Tuning b` indicates to perform only software identification
				![[Pasted image 20260225143212.png]]
				- Reveals the following:
					- IPs (134.209.24.248)
					- Server Technology (Apache/2.4.41 (Ubuntu))
					- WordPress presence (including login page at `/wp-login.php`)
					- Information Disclosure (presence of `license.txt`)
					- Headers (missing `Strict-Transport-Security` header and insecure `x-redirect-by` header)
	- Fingerprinting Lab
		- Questions:
			1. Determine the Apache version running on app.inlanefreight.local on the target system. (Format: 0.0.0)
				1. Add `app.inlanefreight.local` and `dev.inlanefreight.local` into `/etc/hosts` file, pointing to `10.129.42.195`
				2. Run `curl -I app.inlanefreight.local`
					![[Pasted image 20260225143808.png]]
				3. Answer is 2.4.41
			2. Which CMS is used on app.inlanefreight.local on the target system? Respond with the name only, e.g., WordPress.
				1. Open Firefox and go to `app.inlanefreight.local`
				2. Open Wappalyzer
					![[Pasted image 20260225144843.png]]
				3. Answer is Joomla
			3. On which operating system is the dev.inlanefreight.local webserver running in the target system? Respond with the name only, e.g., Debian.
				1. Run `curl -I dev.inlanefreight.local`
					![[Pasted image 20260225144950.png]]
				2. Answer is Ubuntu
- Crawling
	- Crawling, often called Spidering, is the automated process of browsing a website's content and links.
	- Two strategies:
		- Breadth-first Crawling
			- Crawls all links on the seed page, before moving to the next level
		- Depth-first Crawling
			- Crawls from the first link until the last link, before moving back to the seed page for the second link
	- Information that can be extracted:
		- Internal and External Links
		- Comments
			- On blogs, forums or other interactive pages where users might reveal information
		- Metadata
		- Sensitive Files
			- Such as backup files, config files, log files, API keys, credentials, encryption keys
	- Robots.txt
		- Text file placed in the root directory of a website that dictates how web crawlers should behave in the website.
		- Each record consists of:
			- `User-agent`
				- Refers to type of bot. `*` means all bots. `Googlebot` and `Bingbot` for Google and Microsoft respectively
			- `Directives`
				- Instructions that the user-agent shall follow
				- Common directives are:
					- Disallow
						- Robot should not crawl specified path or pattern
					- Allow
						- Permits the bot to crawl the specified path or pattern
					- Crawl-delay
						- Sets a delay (in seconds) between successive requests from bot, to avoid overloading server
					- Sitemap
						- Provides the URL to an XML sitemap for more efficient crawling
		- Rogue bots can still ignore robot.txt file
		- Information that can be gathered by looking at robots.txt file:
			- Uncovering hidden directories
				- Look at `Disallow` paths
			- Mapping website structure
				- Analyzing `Allow` and `Disallow` paths can reveal the website's directory structure.
			- Detecting crawler traps
				- Some websites have "honeypot" directories intended to capture the bot and trigger alerts
	- Well-known URIs
		- Defined in [RFC 8615](https://datatracker.ietf.org/doc/html/rfc8615), a `.well-known` is a directory within the website's root domain that centralizes website's critical metadata, including config files and information related to services, protocols, and security mechanisms
		- Internet Assigned Numbers Authority (IANA) maintains a [registry](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml) of `.well-known` URIs.
		- Notable examples:
			- security.txt
				- Contains contact information for security researchers to report vulnerabilities
				- RFC 9116
			- /.well-known/change-password
				- Standard URL for directing users to a password change page
			- openid-configuration
				- Configuration details for OpenID Connect, an identity layer on top of OAuth 2.0 protocol
			- assetlinks.json
				- Verifying ownership of digital assets in the domain
			- mta-sts.txt
				- Specifies policy for SMTP MTA Strict Transport Security to enhance email security
				- RFC 8461
		- .well-known URIs can reveal endpoints and configuration details
	- Popular Web Crawlers:
		- Burp Suite Spider
		- OWASP ZAP (Zed Attack Proxy)
		- Scrapy (Python framework)
			1. Install Scrapy framework using `pip3 install scrapy`
			2. Download `ReconSpider` using `wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip`
			3. Extract zip file using `unzip ReconSpider.zip`
			4. Run the spider using `python3 ReconSpider.py http://inlanefreight.com`
			5. View results using `nano results.json`
		- Apache Nutch (Scalable Crawler)
	- Crawling Lab
		- Questions
			1. After spidering inlanefreight.com, identify the location where future reports will be stored. Respond with the full domain, e.g., files.inlanefreight.com.
				1. View results of spidering using `nano results.json`
				2. Look for `future reports`
					![[Pasted image 20260225152130.png]]
				3. Answer is inlanefreight-comp133.s3.amazonaws.htb
- Search Engine Discovery
	- Common search operators:
		- `site:`
			- Limits result to specific website or domain
			- Can find publicly accessible pages
		- `inurl:`
			- Finds pages with specific term in URL
			- Can find login pages if its in the URL
		- `filetype:`
			- Finds for specified file type such as pdf, xml, csv, docx
		- `intitle:`
			- Finds pages with specific term in the title
		- `intext:` or `inbody:`
			- Finds pages with terms present in the body of the web page
		- `cache:`
			- Displays cached version of a web page (if available) to see previous content
		- `link:`
			- Finds pages that link to specified website
		- `related:`
			- Find websites related to the specified website
		- `info:`
			- Provides summary of information about website
		- `define:`
			- Provides definitions of a word or phrase
		- `numrange:`
			- Searches for pages containing numbers within specified range
		- `allintext:`
			- Finds pages containing all specified words in body text
		- `allinurl:`
			- Finds pages containing all specified words in URL
		- `allintitle:`
			- Finds pages containing all specified words in title
		- `AND`
			- Narrow results by requiring all operators to be present
		- `OR`
			- Broadens results by including pages with any of the operators
		- `NOT`
			- Excludes results containing specified operator
		- `*`
			- Wilcard; Represents any character or word
		- `..`
			- Range search; finds results within specified numerical range
		- `" "`
			- Searches for exact phrases
		- `-`
			- Excludes terms from the search results
	- Operators in practice:
		- Find login pages
			- `site:example.com inurl:login`
			- `site:example.com (inurl:login OR inurl:admin)`
		- Indentifying exposed files
			- `site:example.com filetype:pdf`
			- `site:example.com (filetype:xls OR filetype:docx)`
		- Uncovering configuration files
			- `site:example.com inurl:config.php`
			- `site:example.com (ext:conf OR ext:cnf)` (searches for extensions commonly used for configuration files)
		- Locating database backup
			- `site:example.com inurl:backup`
			- `site:example.com filetype:sql`
- Web Archives
	- Use `The Wayback Machine` to revisit archived webpages for information
	- Prioritizes websites deemed to be of cultural, historical, or research value, and website owners can request their content to be excluded from the Wayback Machine.
	- How does the Wayback Machine work?
		1. Crawling
			- Uses automated web crawlers to crawl websites and download copies of the webpages
		2. Archiving
			- Downloaded webpages, including images, stylesheets, and scripts, are stored in the archive along with date of snapshot
		3. Accessing
			- Anyone can access a website's snapshot
	- Web Archive Lab
		- Questions:
			1. How many Pen Testing Labs did HackTheBox have on the 8th August 2018? Answer with an integer, eg 1234.
				1. Open https://www.hackthebox.eu/en on 8th August 2018
				2. Explore webpage to find number of machines
					![[Pasted image 20260225154951.png]]
				3. Answer is 74
			2. How many members did HackTheBox have on the 10th June 2017? Answer with an integer, eg 1234.
				1. Open https://www.hackthebox.eu/en on 10th June 2017
				2. Explore webpage to find number of members
					![[Pasted image 20260225154855.png]]
				3. Answer is 3054
			3. Going back to March 2002, what website did the facebook.com domain redirect to? Answer with the full domain, eg http://www.facebook.com/
				1. Open http://www.facebook.com/ on March 2002
				2. Analyze redirected domain
					![[Pasted image 20260225155157.png]]
				3. Answer is http://site.aboutface.com/
			4. According to the paypal.com website in October 1999, what could you use to "beam money to anyone"? Answer with the product name, eg My Device, remove the ™ from your answer.
				1. Open paypal.com on October 1999
				2. Explore webpage to find text with "beam money to anyone"
					![[Pasted image 20260225155411.png]]
				3. Answer is Palm 0rganizer
			5. Going back to November 1998 on google.com, what address hosted the non-alpha "Google Search Engine Prototype" of Google? Answer with the full address, eg http://google.com
				1. Open google.com on November 1998
				2. Copy redirect link of text "Google Search Engine Prototype"
					![[Pasted image 20260225155547.png]]
				3. Answer is http://google.stanford.edu/
			6. Going back to March 2000 on www.iana.org, when exactly was the site last updated? Answer with the date in the footer, eg 11-March-99
				1. Open www.iana.org on March 2000
				2. Look for mentioned of "Page Updated"
					![[Pasted image 20260225155713.png]]
				3. Answer is 17-December-99
			7. According to the wikipedia.com snapshot taken in March 2001, how many pages did they have over? Answer with the number they state without any commas, eg 2000 not 2,000
				1. Open www.wikipedia.com March 2001
				2. Look for mention of "pages"
					![[Pasted image 20260225155826.png]]
				3. Answer is 3000
- Automating Recon
	- Recon Frameworks:
		- [FinalRecon](https://github.com/thewhiteh4t/FinalRecon)
			- Python-based recon tool for SSL certificate checking, WHOIS information gathering, header analysis, and crawling
			- Install using the script below:
				1. Download using `git clone https://github.com/thewhiteh4t/FinalRecon.git`
				2. Change directory `cd FinalRecon`
				3. Install using `pip3 install -r requirements.txt`
				4. Change permission using `chmod +x ./finalrecon.py`
				5. Explore help options using `./finalrecon.py --help`
			- Notable commands:
				- `--url`
				- `--sslinfo`
				- `--whois`
				- `--crawl`
				- `--dns`
				- `--sub`
				- `--dir`
				- `--wayback` (retrieves Wayback URLs)
				- `--ps` (performs fast port scan)
				- `--full` (performs full recon)
		- [Recon-ng](https://github.com/lanmaster53/recon-ng)
			- Python-based recon tool for DNS enumeration, subdomain discovery, port scanning, web crawling, and exploiting known vulnerabilities
		- [theHarvester](https://github.com/laramies/theHarvester)
			- Python-based recon tool for gathering email address, subdomains, hosts, employee names, open ports, and banners using search engines, PGP key servers, and Shodan
		- [SpiderFoot](https://github.com/smicallef/spiderfoot)
			- Can perform DNS lookups, web crawling, port scanning, information gathering of IP addresses, domain names, email addresses, and social media profiles
		- [OSINT Framework](https://osintframework.com/)
			- Collection of various tools for gathering information from social media, search engine, public records
- Skills Assessment
	 - Apply a variety of skills learned in this course, including:
		- Using whois
		- Analysing robots.txt
		- Performing subdomain bruteforcing
		- Crawling and analysing results
	- Pre-requisite: Add `154.57.164.64` pointing to `inlaenefreight.htb` to the `/etc/hosts` file
	- Questions:
		1. What is the IANA ID of the registrar of the inlanefreight.com domain?
			1. Run `whois inlanefreight.com`
			2. Look for mention of `IANA ID`
				![[Pasted image 20260225162005.png]]
			3. Answer is 468
		2. What http server software is powering the inlanefreight.htb site on the target system? Respond with the name of the software, not the version, e.g., Apache.
			1. Open Firefox and point to `http://inlanefreight.htb:32537/robots.txt`
			2. Look for web server software
				![[Pasted image 20260225164922.png]]
			3. Answer is nginx
		3. What is the API key in the hidden admin directory that you have discovered on the target system?
			1. Enumerate vhosts using `gobuster vhost -u http://inlanefreight.htb:31367 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 200 -k --timeout 15s`
				![[Pasted image 20260225211139.png]]
			2. Add vhost `web1337.inlanefreight.htb` to `/etc/hosts`
			3. Use curl to get robots.txt `curl http://web1337.inlanefreight.htb:31367/robots.txt`
				![[Pasted image 20260225211041.png]]
			4. Open the folder `curl -I http://web1337.inlanefreight.htb/admin_h1dd3n`
				![[Pasted image 20260225212343.png]]
			5. Open the folder `curl http://web1337.inlanefreight.htb/admin_h1dd3n/`
				![[Pasted image 20260225212406.png]]
			6. Answer is e963d863ee0e82ba7080fbf558ca0d3f
		4. After crawling the inlanefreight.htb domain on the target system, what is the email address you have found? Respond with the full email, e.g., mail@inlanefreight.htb.
			1. Enumerate vhosts on `web1337` subdomain using `gobuster vhost -u http://web1337.inlanefreight.htb:31367 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 200 -k --timeout 15s`
				![[Pasted image 20260225213736.png]]
			2. Add `dev.web1337` vhost in `/etc/hosts` file
			3. Use ReconSpider using `python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:31367`
			4. Open output using `cat results.json`
			5. Look for email entries
				![[Pasted image 20260225213823.png]]
			6. Answer is 1337testing@inlanefreight.htb
		5. What is the API key the inlanefreight.htb developers will be changing too?
			1. In the `results.json` file, look for mention of API key
				![[Pasted image 20260225213906.png]]
			2. Answer is ba988b835be4aa97d068941dc852ff33



