# Project Codeverse: Red Team (Attackers)

This repository includes a lab ready for a simulated penetration test. The README below contains role assignments, scenario setup, tool commands and full reports for the Red Team (Attackers).

---

## Role Assignment Sheet

### Red Team (Attackers)

o Red Team (9 students)

 Attacker (Team Lead): The team leader whose name is Oniye Ahmad Omotosho coordinated attack workflow, approved tools and also managed the timeline.

 Recon Analysts: Ibrahim Asia and Olayiwola Mubaraq Tomilola performed OSINT, subdomain discovery and directory enumeration.

 Scanner/Enumeration Specialists: Vulnerability scans and map endpoints were ran by Adesina Jamiu and Omijie Mitchel Akhamioje.

 Exploit Handler: Omotosho Olasunkanmi tested for vulnerabilities and also attempted exploitation ethically.

 Documentation & Evidence Analysts: Records, screenshots, notes, payloads and results were compiled by Adetunji Pelumi Gladys and Yahaya Olayinka Abdullah.

---

## Penetration Testing Setup

• Scenario Title: Operation CodeVerse breach – Simulated attack.  
• Environment: Controlled, permission-granted web  
• Prerequisite: Logging enabled (NGINX/Apache access logs, app error logs), dummy test accounts.  
• Example planted weaknesses: Weak login, invalidated inputs, exposed directories, debug mode open, weak admin password, no rate-limiting, robots.txt paths.

---

## Objectives

We are to perform a structured pentest on zonetransfer.me site, discover and safely exploit vulnerabilities, collect proofs and document findings. We did not do any destructive attack or Dos.

---

## Rules of Engagement (ROE)

• Only approved tools were used.  
• We are to attack within the given domain/ip.  
• With each step, we are to record every action and also gave evidence.  
• We are also to submit final reports.

---

## Tools and Commands

### 1. Reconnaissance

Reconnaissance is the process of gathering information about a target system, network or organization to understand it and identify possible weaknesses before an attack or security test. Here are the codes we used to perform reconnaissance;

o whois - It was used to check the domain ownership and information.

```
whois zonetransfer.me
```

o nslookup – It was used to query DNS servers to find information about a domain name, such as it’s Ip address, mail servers, or other DNS records

```
nslookup zonetransfer.me
```

o dig: It is used to query DNS servers and get detailed information about domain names, such as IP addresses, name servers, mail servers, and DNS records

```
dig zonetransfer.me
```

o subfinder - It is used for subdomain discovery

```
subfinder –d zonetransfer.me
```

---

### 2. Scanning & Enumeration

Scanning is the process of actively checking a target system or network to find live hosts, open ports and running services. Enumeration is the deeper process of extracting detailed information from those services, such as usernames, shared resources, network details and system information.

o nmap – It is used to scan for open ports and running services.

```
nmap zonetransfer.me
```

o Service and version detection

```
nmap -sV – to determine open ports and it’s version.
```

o Aggressive scan

```
nmap -A – it is used for aggressive scanning
```

---

### 3. Exploitation

This site was prone to “ ‘  or  1 = 1 – “ sql injection.

---

### 4. Documentation

For every test record:

o URL was tested, payload was used, we had expected vs actual and also we took screenshots of what we got to keep as evidence.


