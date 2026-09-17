# Networkwalks WK2 — Penetration Testing & Security Assessment

**Cybersecurity Internship | Networkwalks | Batch B082**

> Authorized penetration-testing and external attack-surface reconnaissance exercise focused on DNS enumeration, OSINT, web technology fingerprinting, HTTP analysis, WAF detection, and infrastructure mapping.

## 📌 Project Overview

This repository documents my **Week 2 cybersecurity internship assignment with Networkwalks**, involving an authorized penetration-testing and security assessment exercise.

The assessment focused primarily on **reconnaissance and attack-surface discovery** against `networkwalks.com`, with additional reconnaissance of my own local LAN environment.

The objective was to identify publicly discoverable infrastructure, technologies, DNS records, services, and other information that could contribute to an organization's external attack surface.

**Assessment date:** 16 September 2026
**Program/Batch:** B082
**Assessment type:** Authorized security assessment
**Primary target:** `networkwalks.com`

## ⚠️ Authorization & Ethical Scope

Testing was conducted as an authorized cybersecurity learning and penetration-testing exercise.

The assessment was limited to reconnaissance and attack-surface discovery.

No attempt was made to:

* Obtain unauthorized access
* Steal credentials
* Modify data
* Disrupt services
* Perform denial-of-service attacks
* Conduct destructive testing

Any future vulnerability validation should be performed only under explicit written authorization and an agreed Rules of Engagement.

## 🎯 Objectives

The assessment aimed to:

1. Identify publicly discoverable Networkwalks infrastructure.
2. Enumerate DNS records and subdomains.
3. Identify hosting and network infrastructure.
4. Fingerprint publicly exposed web technologies.
5. Identify potentially unnecessary information disclosure.
6. Assess DNS configuration from an external perspective.
7. Identify defensive technologies such as WAF protection.
8. Map relationships between domains, IP addresses, netblocks, DNS records, and autonomous systems.
9. Identify areas requiring further security validation.
10. Develop prioritized security-improvement recommendations.

## 🔎 Methodology

The assessment followed a controlled reconnaissance methodology:

| Phase | Activity                       | Tools              |
| ----- | ------------------------------ | ------------------ |
| 1     | Domain reconnaissance          | WHOIS              |
| 2     | DNS enumeration                | DNSRecon, nslookup |
| 3     | Web technology fingerprinting  | WhatWeb            |
| 4     | HTTP response analysis         | cURL               |
| 5     | WAF detection                  | WAFW00F            |
| 6     | OSINT & infrastructure mapping | Maltego            |
| 7     | Network reconnaissance         | Nmap / Zenmap      |
| 8     | Security interpretation        | Manual analysis    |

## 🛠️ Tools Used

* **Kali Linux** — Security assessment platform
* **WHOIS** — Domain and registration reconnaissance
* **nslookup** — DNS resolution
* **DNSRecon** — DNS enumeration
* **WhatWeb** — Web technology fingerprinting
* **cURL** — HTTP/HTTPS response analysis
* **WAFW00F** — Web Application Firewall detection
* **Maltego** — OSINT and infrastructure relationship mapping
* **Nmap / Zenmap** — Network reconnaissance

## 📊 Key Findings

The reconnaissance identified several externally observable security considerations.

### 1. Public DNS & Service Exposure

Multiple DNS names were discovered, including web, mail, hosting-management and file-management related services.

Examples included:

* `www.networkwalks.com`
* `mail.networkwalks.com`
* `ftp.networkwalks.com`
* `cpanel.networkwalks.com`
* `whm.networkwalks.com`
* `webmail.networkwalks.com`
* `webdisk.networkwalks.com`

These records provide an external observer with information about potentially available services.

**Assessment:** Medium information/attack-surface exposure.

### 2. Web Technology Fingerprinting

WhatWeb identified publicly observable technologies including:

* Apache
* WordPress 7.1
* WordPress Download Manager 3.3.58
* jQuery 3.7.1
* Bootstrap
* Google Tag Manager
* HTML5
* Open Graph Protocol

Technology fingerprinting does not establish a vulnerability by itself, but it provides information that can assist subsequent authorized vulnerability research.

**Assessment:** Medium — Technology Fingerprinting / Information Disclosure.

### 3. WordPress REST API Exposure

HTTP analysis identified publicly referenced WordPress REST API endpoints, including:

`/wp-json/`

The presence of the WordPress REST API is a normal feature and is **not automatically a vulnerability**. Further authorized testing would be required to determine whether any sensitive information is unnecessarily exposed.

**Assessment:** Low — Information Disclosure.

### 4. Infrastructure Concentration

Several discovered DNS entities were associated with:

`192.232.216.135`

The assessment identified this as an architectural consideration because multiple services may depend upon shared infrastructure.

This should not be interpreted as a confirmed single point of failure.

**Assessment:** Medium — Infrastructure Concentration.

### 5. DNS Software Version Disclosure

The assessment identified:

`BIND 9.16.23-RH`

Version disclosure can assist reconnaissance, although disclosure alone does not establish that the DNS infrastructure is vulnerable.

**Assessment:** Low — Information Disclosure.

### 6. localhost DNS Record

The following record was identified:

`localhost.networkwalks.com → 127.0.0.1`

A public hostname resolving to a loopback address is unusual and should be reviewed to determine whether it is intentional.

**Assessment:** Low — Configuration Review Required.

### 7. Administrative Infrastructure Information

An infrastructure-related administrative email address was identified during OSINT analysis.

The report recommends minimizing unnecessary exposure of infrastructure-specific administrative addresses and strengthening mailbox security with controls such as MFA and appropriate email authentication.

**Assessment:** Low–Medium — Information Exposure.

### 8. Suspicious / Malformed Domains

OSINT analysis identified domains resembling the legitimate Networkwalks domain.

These domains were **not classified as malicious solely from the reconnaissance evidence**. Ownership, registration information, DNS relationships, content, and threat-intelligence sources would need to be validated.

**Assessment:** Potential Medium — Requires Validation.

## 🛡️ Positive Security Controls Identified

The assessment also identified several defensive controls:

### ModSecurity WAF

WAFW00F detected a **ModSecurity (SpiderLabs) Web Application Firewall** protecting the web application.

### HTTPS

Direct testing successfully accessed the website over HTTPS.

### HTTP → HTTPS Redirect

HTTP traffic was observed redirecting to HTTPS.

### DNS Zone Transfer Protection

AXFR and IXFR requests were refused by the authoritative DNS servers.

This indicates that unauthorized DNS zone transfer was not observed during the assessment.

### WHOIS Privacy

Registration information included privacy protection, reducing direct exposure of registrant information.

## 📋 Risk Register

| ID   | Finding                                  | Risk              | Status                    |
| ---- | ---------------------------------------- | ----------------- | ------------------------- |
| F-01 | Public technology fingerprinting         | Medium            | Confirmed observation     |
| F-02 | Multiple DNS/service records exposed     | Medium            | Confirmed observation     |
| F-03 | Infrastructure concentration             | Medium            | Architectural observation |
| F-04 | DNS software version disclosure          | Low               | Confirmed observation     |
| F-05 | WordPress REST API publicly referenced   | Low               | Confirmed observation     |
| F-06 | Administrative/root-style email exposure | Low–Medium        | Confirmed observation     |
| F-07 | localhost DNS record                     | Low               | Requires validation       |
| F-08 | DNSSEC reported unsigned                 | Low               | Configuration observation |
| F-09 | Suspicious/malformed domains             | Medium potential  | Requires validation       |
| F-10 | HTTPS/SSL status discrepancy             | —                 | Requires validation       |
| F-11 | DNS zone transfer                        | No issue observed | AXFR/IXFR refused         |
| F-12 | WAF protection                           | Positive control  | ModSecurity detected      |

## 🔧 Recommended Improvements

Based on the reconnaissance findings, the report recommends:

### Priority 1 — Immediate Review

* Perform an authoritative inventory of public DNS records.
* Remove obsolete or unnecessary DNS records.
* Investigate suspicious/typo-similar domains.
* Review Internet-exposed administrative services.
* Restrict administrative interfaces using appropriate access controls.

### Priority 2 — Hardening

* Keep WordPress and plugins updated.
* Remove unused themes and plugins.
* Maintain supported PHP and web-server versions.
* Require MFA for administrative systems.
* Review SPF, DKIM and DMARC configuration.
* Restrict administrative interfaces from unrestricted Internet access where practical.

### Priority 3 — Monitoring

Monitor for:

* New DNS records
* New subdomains
* Certificate changes
* Unexpected IP changes
* New open ports
* Suspicious authentication activity
* WordPress/plugin changes
* WAF alerts
* Phishing domains
* Domain impersonation

## 🧪 Recommended Next Testing Phase

The current work is primarily reconnaissance.

A subsequent authorized assessment could include:

### Web Application Security

* Authentication testing
* Authorization testing
* Session-management testing
* Input-validation testing
* OWASP Top 10 assessment
* WordPress security assessment
* Plugin vulnerability assessment

### Network Security

* TCP/UDP service enumeration
* Service/version identification
* TLS configuration assessment
* External attack-surface validation
* Administrative-interface exposure assessment

### DNS Security

* Complete DNS inventory
* DNSSEC validation
* DNS configuration review
* Subdomain takeover checks where applicable
* Zone-transfer validation
* Dangling DNS record checks

### Email Security

* SPF validation
* DKIM validation
* DMARC validation
* Mail-server configuration review
* Email authentication testing

## 💻 Commands Used

```bash
# DNS Resolution
nslookup networkwalks.com

# DNS Enumeration
dnsrecon -d networkwalks.com

# WAF Detection
wafw00f networkwalks.com

# HTTP Header Analysis
curl -I https://networkwalks.com

# Web Technology Fingerprinting
whatweb networkwalks.com

# WHOIS Enumeration
whois networkwalks.com
```


## 📄 Full Assessment Report

The complete 30-page assessment report is available in the `report/` directory.

The report contains the detailed methodology, technical observations, evidence interpretation, risk register, recommendations, and professional tester statement.

## 👨‍💻 Author

**Adongo Peter Oduor**

Cybersecurity & Ethical Hacking — Batch B082
Networkwalks Internship

## 📚 Learning Outcomes

Through this assignment, I gained practical experience in:

* External attack-surface reconnaissance
* DNS enumeration
* OSINT
* Web technology fingerprinting
* HTTP response analysis
* WAF identification
* Infrastructure mapping
* Security-risk interpretation
* Technical security documentation
* Ethical and authorized penetration-testing methodology

---

**Disclaimer:** This repository documents an authorized cybersecurity learning exercise. The information presented should not be used to conduct unauthorized testing against the referenced infrastructure. Any security testing should be performed only with explicit authorization and within an agreed scope and Rules of Engagement.

