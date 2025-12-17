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


# RED TEAM – PENETRATION TEST REPORT

## Project Title: Operation CodeVerse Breach

**Members:**
Oniye Ahmad Omotosho
Ibrahim Asia
Olayiwola Mubaraq Tomilola
Adesina Jamiu
Omijie Mitchel Akhamioje
Omotosho Olasunkanmi
Yahaya Olayinka Abdullah
Adetunji Pelumi Gladys

**Date:** Tuesday, 16th December 2025.

---

## Introduction

This project documents a DNS reconnaissance and enumeration exercise conducted as our cybersecurity/ethical hacking practical project. The focus of the exercise is to demonstrate how improper DNS configuration—specifically unrestricted zone transfers—can expose sensitive organizational information. The target domain, zonetransfer.me, was our authorized website.

---

## Executive Summary

A full DNS zone transfer was successfully performed against the authoritative name server nsztm1.digi.ninja for the domain zonetransfer.me. The successful AXFR response revealed 47 DNS records, including hostnames, IP addresses, email infrastructure, internal subdomains, service records, and sensitive TXT records. In a real-world scenario, this level of disclosure would represent a critical security misconfiguration, significantly aiding reconnaissance and subsequent attack phases.

---

## Scope of Testing

* **Target Domain:** zonetransfer.me
* **Authoritative Name Server:** nsztm1.digi.ninja
* **Assessment Type:** DNS Reconnaissance and Enumeration
* **Technique Used:** DNS Zone Transfer (AXFR)
* **Tool Used:** dig (Domain Information Groper)

This assessment was limited strictly to DNS enumeration and did not involve exploitation or active attacks on services.

---

## Methodology

The following command was executed to request a full DNS zone transfer:

```
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

This command requests the entire DNS database (zone file) from the authoritative server. Properly secured DNS servers should deny such requests from unauthorized clients. In this case, the request was accepted.

---

## Reconnaissance Results

### DNS Administration Information

* **Primary Name Server:** nsztm1.digi.ninja
* **Administrator Email:** [robin@digi.ninja](mailto:robin@digi.ninja)
* **Zone Serial Number:** 2014101601

NB: This information reveals who manages the DNS zone and provides metadata useful for timing updates and changes.

### Name Servers (NS Records)

* nsztm1.digi.ninja
* nsztm2.digi.ninja

These are the authoritative servers responsible for the domain.

### IP Address Mapping (A / AAAA Records)

* [www.zonetransfer.me](http://www.zonetransfer.me) → 217.147.180.162
* office.zonetransfer.me → 4.23.39.254
* vpn.zonetransfer.me → 174.36.59.154
* owa.zonetransfer.me → 207.46.197.32
* IPv6 records such as deadbeef.zonetransfer.me and ipv6actnow.org.zonetransfer.me

This allows an attacker to directly identify live hosts and services.

### Email Infrastructure and Emails Obtained

* The domain uses Google Mail (ASPMX) servers for email delivery.
* Email-related NAPTR records were present.

Email addresses discovered:

* [robin@digi.ninja](mailto:robin@digi.ninja)
* [pippa@zonetransfer.me](mailto:pippa@zonetransfer.me)

Email information can be leveraged for phishing and social engineering attacks.

---

### Internal and Sensitive Subdomains

Several internal or sensitive-looking subdomains were exposed:

* internal.zonetransfer.me
* intns1.zonetransfer.me
* intns2.zonetransfer.me
* staging.zonetransfer.me
* vpn.zonetransfer.me

These significantly aid attacker reconnaissance.

---

### Service Discovery Records

* SRV and NAPTR records disclosed SIP and email service configurations.
* These records provide insight into service priorities, ports, and protocols.

Such records help attackers understand how internal services are structured

---

### TXT Records and Information Leakage

Multiple TXT records were found containing:

* Google site verification data
* Internal notes and contact reminders
* Common attack test strings, including:

  * SQL Injection payloads
  * XSS payloads
  * Shellshock payloads

TXT records are often overlooked and can unintentionally leak sensitive or operational information.

* LOC record: Reveals geographic coordinates
* PTR record: Reverse DNS mapping
* RP record: Responsible person mapping

These records further enrich reconnaissance data.

---

## Findings from Scanning and Enumeration

The DNS zone transfer revealed the entire DNS configuration of the domain, resulting in the following key findings:

* **Successful DNS Zone Transfer:**
  The authoritative name server allowed an AXFR request, exposing all DNS records for the domain. This indicates a DNS misconfiguration.

* **Administrative Information Disclosure:**
  The SOA record revealed the primary DNS server and the administrator’s email address ([robin@digi.ninja](mailto:robin@digi.ninja)).

* **Exposed Email Infrastructure:**
  Multiple MX records showed that the domain uses Google Mail servers. Additional email-related records exposed contact email addresses such as [pippa@zonetransfer.me](mailto:pippa@zonetransfer.me).

* **Internal and Sensitive Subdomains Identified:**
  Subdomains such as internal, vpn, staging, office, and owa were discovered, indicating internal systems, remote access services, and testing environments.

* **IP Address Enumeration:** Numerous IPv4 and IPv6 addresses were mapped to subdomains, allowing identification of live hosts and services.

* **Service Discovery Information:** SRV and NAPTR records disclosed SIP and email service configurations, including service priorities and ports.

* **Information Leakage via TXT Records:** TXT records contained internal notes, verification strings, and test payloads related to SQL injection, XSS, and Shellshock, demonstrating poor information handling practices.

* **Additional Metadata Exposure:** LOC records revealed geographic location data, while PTR and RP records provided reverse DNS and responsible person details.

---

## One-Line Summary

The DNS zone transfer exposed internal subdomains, IP addresses, email infrastructure, service configurations, and sensitive metadata, providing a complete blueprint of the target’s DNS environment.


## Vulnerability Identification

The DNS zone transfer revealed critical security weaknesses:

* Unrestricted AXFR: The authoritative name server allowed a full zone transfer, exposing all DNS records.
* Information Disclosure: Administrative emails, internal subdomains, IP addresses, and network structure were exposed.
* Internal Infrastructure Exposure: Subdomains like internal, vpn, staging, and office indicate internal and remote systems.
* Email Exposure: MX records and email addresses could be targeted for phishing or social engineering.
* Sensitive Data in TXT Records: Internal notes and test payloads (SQLi, XSS, Shellshock) were publicly accessible.
* Service Enumeration: SRV and NAPTR records revealed active communication services.
* Metadata Leakage: LOC, PTR, and RP records disclosed location and administrative details.

**Summary:** The domain’s DNS configuration allows full reconnaissance and exposes critical internal information due to misconfigured zone transfers.

---

## Exploitation Attempts

Analysis of the DNS zone transfer output revealed multiple exploitation-related payloads stored in TXT records. These payloads were identified through passive enumeration and were not actively executed.

### Payloads Identified

* **SQL Injection (SQLi):**

```
' or 1=1 --
```

* **Cross-Site Scripting (XSS):**

```
'><script>alert('Boo')</script>
```

**Shellshock Vulnerability:**

```
() { :]}; echo ShellShocked
```

**Command Injection Indicator:**

```
; ls
```

---

### Outcomes

* Payloads were successfully discovered due to an unrestricted DNS zone transfer.
* No exploitation was performed; findings are based on information disclosure only.
* The exposed payloads indicate potential attack vectors that could be used if vulnerable services are present.

---

### Security Impact

* Attackers can plan exploitation without interacting directly with the target.
* Presence of test payloads in public DNS records reflects poor security hygiene.
* Increases the likelihood of successful attacks in a real-world environment.

**Summary:** DNS enumeration exposed multiple exploitation payloads, demonstrating how misconfigured DNS can assist attackers during the exploitation planning phase.

---

## Risk Rating

**Risk Rating:** Based on the DNS zone transfer results, the overall risk level is rated as **HIGH**.

### Risk Justification

* **Unrestricted DNS Zone Transfer:** Allows any external user to retrieve the full DNS zone, exposing internal and sensitive infrastructure details.
* **Exposure of Exploitation Payloads:** Publicly accessible TXT records contain SQLi, XSS, Shellshock, and command injection payloads, which aid attackers in planning exploits.
* **Internal System Disclosure:** Subdomains related to internal networks, VPN, staging, and office systems were revealed.
* **Email and Service Exposure:** Email infrastructure and service discovery records increase the risk of phishing and targeted attacks.

---

### Impact

* Significant information disclosure
* Reduced effort required for exploitation
* Increased likelihood of successful, targeted attacks

**Final Assessment:** The combination of unrestricted AXFR access and sensitive data exposure through DNS records constitutes a High-risk security issue.

---

## Mitigation Recommendations

To reduce the security risks identified from the DNS zone transfer, the following mitigation measures are recommended:

1. Restrict Zone Transfers

   * Allow AXFR requests only from authorized secondary DNS servers.
   * Deny zone transfer requests from untrusted external sources.

2. Audit and Harden DNS Configuration

   * Regularly review DNS server settings for misconfigurations.
   * Remove unnecessary records and limit exposure of internal hostnames.

3. Avoid Sensitive Data in DNS Records

   * Do not store internal notes, test payloads, or personal contact information in TXT or other publicly accessible DNS records.

4. Monitor and Log DNS Activity

   * Enable logging for all DNS queries and zone transfer attempts.
   * Alert on suspicious or unauthorized AXFR requests.

5. Separate Testing and Production Data

   * Ensure that payloads and test data (SQLi, XSS, Shellshock examples) are not included in production DNS records.

6. Regular Security Assessment

   * Conduct periodic DNS security audits to detect information leakage.
   * Combine with network scanning and vulnerability assessments for proactive mitigation.

> **Summary:** Properly securing DNS configuration and limiting public exposure will prevent attackers from gathering sensitive information and reduce the risk of targeted exploitation.

---

## Conclusion

This assessment demonstrates how a single DNS misconfiguration can lead to extensive information disclosure during the reconnaissance phase of an attack. The unrestricted zone transfer exposed internal subdomains, IP addresses, email infrastructure, service discovery records, and exploitation-related payloads, effectively providing a blueprint of the target environment. Such exposure significantly lowers the barrier for targeted attacks, and service exploitation. Securing DNS configurations, restricting zone transfers, and eliminating sensitive data from public records are critical measures to reduce the attack surface and strengthen overall security posture.

