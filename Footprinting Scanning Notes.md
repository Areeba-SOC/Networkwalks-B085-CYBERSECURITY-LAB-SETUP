# Footprinting & Network Scanning – Methodology Notes

My study notes on **Phase 1 (Reconnaissance & Footprinting)** and **Phase 2 (Scanning & Network Discovery)** of a penetration test, based on authorized educational lab exercises.

> **Disclaimer:** These notes are theory and general methodology only. They contain no real client data. Only test systems you own or have **written permission** to test. Unauthorized scanning can be illegal.

---

## 1. Penetration Testing Phases (overview)

| Phase | Name | Goal |
|---|---|---|
| 1 | Reconnaissance & Footprinting | Collect information about the target |
| 2 | Scanning & Enumeration | Find live hosts, open ports, services |
| 3 | Gaining Access | Exploit weaknesses (authorized only) |
| 4 | Maintaining Access | Test persistence |
| 5 | Reporting & Cleanup | Document findings and fixes |

These notes cover **Phase 1 and 2 only**. No exploitation is involved.

---

## 2. Footprinting Basics

**Footprinting** is collecting as much information as possible about a target before any active testing.

- **Passive:** no direct contact with the target (WHOIS, public records, search engines, OSINT).
- **Active:** direct interaction with the target (DNS queries, HTTP requests, scans).

**Why it matters:** the more an attacker learns here, the easier later phases become. Defenders can use the same techniques to see what they expose.

---

## 3. Tools Used (Kali Linux)

Example target used below: `example.com` (placeholder).

| Tool | Purpose | Example command | What it can reveal |
|---|---|---|---|
| **WHOIS** | Domain registration lookup | `whois example.com` | Registrar, creation/expiry dates, name servers, DNSSEC status |
| **WhatWeb** | Web technology fingerprinting | `whatweb example.com` | Web server, CMS, plugins, JavaScript libraries, headers |
| **nslookup** | DNS resolution | `nslookup example.com` | IP address behind a domain |
| **curl -I** | Read HTTP response headers | `curl -I https://example.com` | Server type, cookies, caching, security headers, API links |
| **wafw00f** | WAF detection | `wafw00f example.com` | Whether a Web Application Firewall is present and which one |
| **DNSRecon** | DNS enumeration | `dnsrecon -d example.com` | NS, MX, TXT, SPF, SRV records |
| **Maltego** | Graph-based OSINT | GUI: add *Domain* entity → run transforms | Relationships between domains, emails, infrastructure |

### 3.1 WHOIS
Public registration data. Useful to understand who manages a domain and which DNS provider it uses. Registrar locks and DNSSEC status show basic domain hygiene.

### 3.2 WhatWeb
Identifies the technologies a site runs. Outdated CMS or plugin versions are a common starting point for deeper (authorized) testing, so version exposure is worth reducing.

### 3.3 nslookup
Converts a domain to an IP. Pointing to a different resolver (for example `8.8.8.8`) gives a **non-authoritative** answer, which comes from a cache rather than the domain's own name server.

### 3.4 curl -I
Fetches headers only. Look at `server`, `set-cookie` (Secure/HttpOnly flags), cache headers and `link` headers, which may reveal API endpoints such as the WordPress REST API path.

### 3.5 wafw00f
Sends normal and slightly unusual requests and compares responses to guess if a WAF is in front of the site. Knowing a WAF exists tells a tester how much filtering to expect.

### 3.6 DNSRecon
Enumerates DNS records:
- **NS / MX** – name servers and mail servers
- **TXT / SPF** – ownership verification and allowed mail senders (`~all` = soft fail, `-all` = hard fail)
- **SRV** – service locations such as email autodiscover

### 3.7 Maltego
Turns raw data into a graph. Starting from one entity (a domain), transforms pull related entities (email addresses, DNS names, IPs), making relationships easy to see.

---

## 4. Network Scanning with Zenmap / Nmap

**Zenmap** is the graphical front end for **Nmap**.

**Steps (on your own network):**
1. Find your IP and subnet: `ipconfig` (Windows) or `ip a` (Linux).
2. Open Zenmap, enter the subnet (example: `192.168.1.0/24`).
3. Choose the **Ping scan** profile (equivalent to `nmap -sn 192.168.1.0/24`).
4. Read the live hosts list in *Nmap Output* / *Hosts*.
5. Open the **Topology** tab to see a map of discovered hosts; use **Save Graphic** to export it.

**Key points**
- A ping scan checks **host availability only** – it does not scan ports.
- MAC addresses are shown only for hosts on the **same local segment**, usually when Nmap runs with admin/root privileges.
- Any unknown device in the results should be investigated.

---

## 5. Defensive Takeaways

1. Review what technology information your website exposes.
2. Keep CMS, plugins and server software updated.
3. Remove unnecessary information from HTTP headers.
4. Review DNS, SPF and mail records regularly; consider DNSSEC.
5. Keep your WAF enabled and tuned.
6. Scan your own network regularly and investigate unknown devices.
7. Document your network topology.

---

## 6. Ethics & Legal

- Get **written authorization** that defines scope and dates.
- Stay inside the scope; anything not listed is out of scope.
- Keep findings confidential unless you have permission to publish.
- Practice legally on your own lab, `scanme.nmap.org` (Nmap's official test host), TryHackMe or HackTheBox.

---

## 7. Skills Practiced

`OSINT` · `DNS` · `HTTP headers` · `Web fingerprinting` · `WAF detection` · `Host discovery` · `Network topology` · `Technical reporting`

## 8. References

- Nmap Reference Guide – https://nmap.org/book/man.html
- Kali Linux Tools – https://www.kali.org/tools/
- Maltego Documentation – https://docs.maltego.com/
