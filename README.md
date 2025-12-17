
This README below contains roles assignments, scenario setup, tool commands and full reports for the Red Team (Attackers).

## Role (Attackers)

**Red Team (9 students)**

• Attacker(Team Lead): Oniye Ahmad Omotosho.

• Recon Analysts: Ibrahim Asia and Olayiwola Mubaraq Tomilola.

• Scanner/Enumeration Specialists: Adesina Jamiu and Omije Mitchel Akhamioje.

• Exploit Handler: Omotosho Olasunkanmi.

• Documentation & Evidence Analysts: Adetunji Pelumi Gladys and Yahaya Olayinka Abdullah.

## Penetration Testing Setup

• Scenario Title: Operation CodeVerse breach – Simulated attack.

• Environment: Controlled, permission-granted web

• Prerequisite: Logging enabled (NGINX/Apache access logs, app error logs), dummy test accounts.

• Example planted weaknesses: Weak login, invalidated inputs, exposed directories, debug mode open, weak admin password, no rate-limiting, robots.txt paths.

## Objectives

```
We are to perform a structured pentest on zonetransfer.me site, discover and safely exploit vulnerabilities, collect proofs and document findings. We did not do any destructive attack or Dos.
```

## Rules of Engagement (ROE)

• Only approved tools were used.

• We are to attack within the given domain/ip.

• With each step, we are to record every action and also gave evidence.

• We are also to submit final reports.

## Tools and commands

### 1. Reconnaissance (Information gathering)

Reconnaissance: Reconnaissance is the process of gathering information about a target system, network or organization to understand it and identify possible weaknesses before an attack or security test. Here are the codes we used to perform reconnaissance;

o whois - It was used to check the domain ownership and information.

```
whois zonetransfer.me
```
![whois](https://github.com/user-attachments/assets/76ac9463-1599-4a9c-a73f-546a40ff7779)
![whois2](https://github.com/user-attachments/assets/95809cc6-c727-41c4-8790-f8d7e069afab)
![whois3](https://github.com/user-attachments/assets/6e5fe5e5-1442-4900-b628-62c1c64da5d2)


o nslookup – It was used to query DNS servers to find information about a domain name, such as it’s Ip address, mail servers, or other DNS records

```
nslookup zonetransfer.me
```

![nslookup](https://github.com/user-attachments/assets/91440641-55bf-4667-a49e-652993805759)


o dig: It is used to query DNS servers and get detailed information about domain names, such as IP addresses, name servers, mail servers, and DNS records

```
dig zonetransfer.me
```

![dig1](https://github.com/user-attachments/assets/f8c4df0e-75da-47ef-8950-3bdd03987d50)
![dig2](https://github.com/user-attachments/assets/b63c81b7-3a7d-44ef-97bc-ea49c9451704)


o subfinder - It is used for subdomain discovery

```
subfinder –d zonetransfer.me
```
![subfinder](https://github.com/user-attachments/assets/696e2019-58c5-461c-baa2-fe8633bfea26)


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

![nmap1](https://github.com/user-attachments/assets/ad27fb47-a46e-4f39-8a7a-1aec2057de80)
![nmap2](https://github.com/user-attachments/assets/78f7abae-a800-443a-80ce-a3c76b9938de)

### 3. Vulnerability Analysis

The DNS zone transfer revealed critical security weaknesses:

• Unrestricted AXFR: The authoritative name server allowed a full zone transfer, exposing all DNS records.

• Information Disclosure: Administrative emails, internal subdomains, IP addresses, and network structure were exposed.

• Internal Infrastructure Exposure: Subdomains like internal, vpn, staging, and office indicate internal and remote systems.

• Email Exposure: MX records and email addresses could be targeted for phishing or social engineering.

• Sensitive Data in TXT Records: Internal notes and test payloads (SQLi, XSS, Shellshock) were publicly accessible.

• Service Enumeration: SRV and NAPTR records revealed active communication services.

• Metadata Leakage: LOC, PTR, and RP records disclosed location and administrative details.

IMAGEEEEEE

### 4. Exploitation

Analysis of the DNS zone transfer output revealed multiple exploitation-related payloads stored in TXT records. These payloads were identified through passive enumeration and were not actively executed.

**Payloads Identified**

* **SQL Injection (SQLi):**

```
' or 1=1 --
```

* **Cross-Site Scripting (XSS):**

```
'><script>alert('Boo')</script>
```

Shellshock Vulnerability:

```
() { :]}; echo ShellShocked
```

Command Injection Indicator:

```
; ls
```

**Outcomes**

* Payloads were successfully discovered due to an unrestricted DNS zone transfer.
* No exploitation was performed; findings are based on information disclosure only.
* The exposed payloads indicate potential attack vectors that could be used if vulnerable services are present.

### 5. Post Exploitation: Security Impact

* Attackers can plan exploitation without interacting directly with the target.
* Presence of test payloads in public DNS records reflects poor security hygiene.
* Increases the likelihood of successful attacks in a real-world environment.

**Summary:** DNS enumeration exposed multiple exploitation payloads, demonstrating how misconfigured DNS can assist attackers during the exploitation planning phase.

**Risk Rating:** Based on the DNS zone transfer results, the overall risk level is rated as HIGH.

**Risk Justification**

Unrestricted DNS Zone Transfer: Allows any external user to retrieve the full DNS zone, exposing internal and sensitive infrastructure details.

Exposure of Exploitation Payloads: Publicly accessible TXT records contain SQLi, XSS, Shellshock, and command injection payloads, which aid attackers in planning exploits.

Internal System Disclosure: Subdomains related to internal networks, VPN, staging, and office systems were revealed.

Email and Service Exposure: Email infrastructure and service discovery records increase the risk of phishing and targeted attacks.

**Impact**

* Significant information disclosure
* Reduced effort required for exploitation
* Increased likelihood of successful, targeted attacks

**Final Assessment:** The combination of unrestricted AXFR access and sensitive data exposure through DNS records constitutes a High-risk security issue.

### 6. Mitigation Recommendations

To reduce the security risks identified from the DNS zone transfer, the following mitigation measures are recommended:

• Restrict Zone Transfers

* Allow AXFR requests only from authorized secondary DNS servers.
* Deny zone transfer requests from untrusted external sources.

• Audit and Harden DNS Configuration

* Regularly review DNS server settings for misconfigurations.
* Remove unnecessary records and limit exposure of internal hostnames.

• Avoid Sensitive Data in DNS Records

* Do not store internal notes, test payloads, or personal contact information in TXT or other publicly accessible DNS records.

• Monitor and Log DNS Activity

* Enable logging for all DNS queries and zone transfer attempts.
* Alert on suspicious or unauthorized AXFR requests.

• Separate Testing and Production Data

* Ensure that payloads and test data (SQLi, XSS, Shellshock examples) are not included in production DNS records.

• Regular Security Assessment

* Conduct periodic DNS security audits to detect information leakage.
* Combine with network scanning and vulnerability assessments for proactive mitigation.

> **Summary:** Properly securing DNS configuration and limiting public exposure will prevent attackers from gathering sensitive information and reduce the risk of targeted exploitation.

### 7. Conclusion

This assessment demonstrates how a single DNS misconfiguration can lead to extensive information disclosure during the reconnaissance phase of an attack. The unrestricted zone transfer exposed internal subdomains, IP addresses, email infrastructure, service discovery records, and exploitation-related payloads, effectively providing a blueprint of the target environment. Such exposure significantly lowers the barrier for targeted attacks, and service exploitation. Securing DNS configurations, restricting zone transfers, and eliminating sensitive data from public records are critical measures to reduce the attack surface and strengthen overall security posture.




