# Introduction
- Where to Scan
	1. IDE Scanning (plugins)
	2. Pull Requests (GitHub Actions)
	3. Scanning Containers (Container Registry)
	4. Scanning Runtime Containers
	5. Runtime Security
- Types of Scanners
	- Software Development Life Cycle (SDLC)
	- Software Composition Analysis (SCA)
	- Static Application Security Testing (SAST)
	- Secret Scanning
	- Container Scanning
	- Infrastructure as Code (IaC)
	- Dynamic Application Security Testing (DAST)
	- Cloud Security Posture Management (CSPM)
# Code Security
- Code file categories to Scan
	- General Information Files:
		- README
		- SECURITY
		- LICENSE
		- CHANGELOG
		- CODEOWNERS
		- .gitignore
	- First-Party Code
		- Actual source code
		- Scanned by SAST
		- Vulnerable to misconfiguration, insecure coding practices
	- Dependency Files
		- Third-party code, downloaded by package managers ("artifactories") usuck as npm, pypi
		- Examples are package.json and requirements.txt, gradle, maven, .gemfile
	- Build Files
		- Loading application into a state for production
		- Can be scanned as an image, or the file itself
		- Examples are Dockerfile
	- Infrastructure files
		- Define IaC before it's deployed
		- Examples are Terraform, .tf, Kubernetes files like Helm
- Static Application Security Testing (SAST)
	- Looks for vulnerable patterns in first-party code
	- Related to Common Weakness Enumeration (CWE)
	- Stops vulnerability from entering code, and remediate existing findings
	- Produces large numbers of false positive. They must be verified and removed before opening ticket, and must work closely with developers to prioritize fixes.
- Software Bill of Materials (SBOM)
	- List of all component parts of the application
	- Mainly used for compliance (especially with US government)
	- Example of SBOM scanner is OWASP Syft
		- How to produce SBOM
			1. Pull docker image locally `docker pull <name of docker file>`
			2. Run scan and save to file using `syft <name of docker file> > sbom.json`
	- SBOM will produce lots of data and large numbers, but often does not indicate a vulnerability
		- Use tools that include Vulnerability Exploitability Exchange (VEX) statements to indicate whether vulnerable or not
		- Some component parts are not reachable by attackers and therefore not vulnerable
	- Useful for on-premise applications, not so useful for SaaS applications
- Software Composition Analysis (SCA)
	- Checks if imported packages are vulnerable (in packages.json)
	- Difference between Direct Dependency versus Transitive Dependency
		- Direct dependency
			- A package that is imported directly
			- Vulnerabilities in direct dependencies are riskier than transitive dependencies
		- Transitive dependency
			- Happens when an imported package, also imports packages of its own.
			- Unusual that transitive dependencies cause as many issues are direct dependency
			- Typically filtered out when prioritizing fixes.
	- False positives can be removed by checking for reachability (use Backslash)
	- It is normal to find more false positives in the results, than vulnerabilities that can be actually exploitable
# Container Security
# Runtime Security
# Remediating Findings
# Conclusion