---
layout: post
title: "Using Nmap for Practical System Administration"
category: blue
---

## Introduction

Nmap gets treated like a hacker tool because that is the framing people like to argue about. That framing is lazy. For a system administrator, Nmap is a visibility tool. It answers boring, operationally important questions: What is alive? What is listening? Did somebody expose a new service? Did the TLS configuration drift? Is the firewall doing what the ticket says it should be doing?

Those questions are not exotic. They are the daily work of keeping an environment from turning into a pile of undocumented exceptions. The anti-pattern is waiting for a vulnerability scanner, a monitoring alert, or a breach report to tell you what was already visible on the network. Nmap gives administrators a quick way to verify reality instead of trusting inventory spreadsheets and good intentions.

## Questions

1. **What makes Nmap useful for normal system administration?**
   Nmap is useful because it turns network assumptions into testable facts. It can discover hosts, identify listening ports, fingerprint services, run safe NSE scripts, and save output in formats that can be compared later.

2. **Is this only for security teams?**
   No. Security teams may use Nmap for reconnaissance, but administrators can use the same tool for change validation, service inventory, firewall testing, certificate review, and troubleshooting. The intent and scope matter.

3. **What should an administrator scan first?**
   Start with networks and systems you own or are authorized to assess. Build a baseline before chasing obscure checks. A simple host discovery scan is often more useful than an aggressive scan run without context.

```bash
# Bad Practice: Starting with an aggressive scan against an entire network
nmap -A 192.0.2.0/24

# Best Practice: Start with host discovery and narrow the scope
nmap -sn 192.0.2.0/24
```

4. **How can Nmap help with asset inventory?**
   Run discovery scans against approved network ranges and compare the results to your CMDB, DHCP leases, cloud inventory, or monitoring platform. Any host that appears on the wire but not in inventory deserves an explanation.

```bash
nmap -sn 192.0.2.0/24 -oA scans/office-discovery-2026-05-21
```

5. **How can Nmap help with service drift?**
   Service drift is when a host slowly stops matching the design. Maybe SSH appears on a database server. Maybe a temporary web listener never got removed. A targeted service scan makes those changes visible.

```bash
# Bad Practice: Trusting that the build standard still matches reality
# "This server should only expose HTTPS, so it probably does."

# Best Practice: Verify the exposed services
nmap -sV --top-ports 100 app01.example.com
```

6. **Where does the Nmap Scripting Engine fit?**
   NSE scripts are useful when the question is deeper than "is the port open?" For example, `ssl-cert` can inspect certificate details, `ssh2-enum-algos` can list SSH algorithms, and `ssl-enum-ciphers` can review TLS support when a noisier TLS check is approved.

```bash
nmap -sV --script ssl-cert,ssl-enum-ciphers -p 443 www.example.com
nmap -sV --script ssh2-enum-algos -p 22 jump01.example.com
```

7. **Can Nmap validate firewall changes?**
   Yes. After a firewall rule change, scan from the same network perspective as the expected client. A scan from your laptop does not validate the same thing as a scan from an application subnet.

```bash
# Bad Practice: Closing the ticket because the firewall rule was entered
# Best Practice: Test the port from the expected source network
nmap -Pn -p 22,80,443 db01.example.com
```

8. **Can Nmap help troubleshoot DNS, NTP, SNMP, and SMB?**
   Yes, as long as you use targeted checks. A sysadmin does not need a noisy "scan everything" habit. Pick the protocol, pick the host, and ask a specific question.

```bash
nmap -sU -p 53 --script dns-recursion dns01.example.com
nmap -sU -p 123 --script ntp-info ntp01.example.com
nmap -sU -p 161 --script snmp-info printer01.example.com
nmap -sV -p 445 --script smb-protocols,smb-security-mode fileserver01.example.com
```

9. **How should Nmap output be handled?**
   Save it. Terminal output is for eyeballs; structured output is for operations. Use normal, grepable, or XML output depending on what you need to do next.

```bash
nmap -sV -p 22,80,443 web01.example.com -oA scans/web01-service-baseline
```

10. **What should administrators avoid?**
    Avoid surprise scans, unauthorized targets, brute-force scripts, denial-of-service scripts, and aggressive options against fragile systems. The point is operational visibility, not proving that production can be made worse at 2:00 PM on a Tuesday.

## Problem / Anti-pattern

The common anti-pattern is treating network exposure as a documentation problem. A diagram says the server has HTTPS open. A ticket says the firewall allows only port 443. A spreadsheet says the printer is on the facilities VLAN. Everyone nods, and nobody checks.

That is how small exceptions become permanent architecture.

Another bad habit is using Nmap only in panic mode. Something breaks, a port scan happens, and the output becomes a one-off troubleshooting artifact that disappears into a terminal scrollback. That helps in the moment, but it does not build operational memory.

Nmap is much more valuable when it becomes part of routine administration:

- After provisioning a server, verify expected exposure.
- After a firewall change, test from the correct source network.
- After a certificate renewal, inspect the served certificate.
- After decommissioning a service, confirm the port is closed.
- On a schedule, compare actual network services against the intended baseline.

## Technical Analysis

There are three practical layers to using Nmap well as an administrator.

The first layer is discovery. This is where `-sn` is useful. It tells you which hosts appear alive without doing a full port scan. It is not perfect, because firewalls and host configurations can block discovery probes, but it is a clean starting point.

```bash
nmap -sn 192.0.2.0/24
```

The second layer is service inventory. This is where `-sV` earns its keep. A port number alone is not enough. Port 443 might be nginx, Apache, a load balancer, a storage appliance, or a forgotten management UI. Service detection helps turn an open port into an actual administrative lead.

```bash
nmap -sV -p 22,80,443,445 host01.example.com
```

The third layer is focused protocol inspection with NSE. This is where administrators should be selective. NSE has powerful scripts, and not all of them are appropriate for routine production checks. For sysadmin work, prefer scripts that inspect and report rather than scripts that brute-force, exploit, or stress a service. Treat noisy scripts, including TLS cipher enumeration, as approved change windows or targeted checks instead of casual background noise.

Useful administrative checks include:

- `ssl-cert` for certificate subject, issuer, validity, and names.
- `ssl-enum-ciphers` for TLS protocol and cipher support, with the caveat that it is noisy.
- `ssh2-enum-algos` for SSH algorithm review.
- `http-title` and `http-headers` for quick web service identification.
- `dns-recursion` for checking whether a DNS server allows recursion.
- `smb-protocols` and `smb-security-mode` for SMB exposure and signing posture.

```bash
nmap -sV --script http-title,http-headers -p 80,443 intranet.example.com
nmap -sV --script ssl-cert,ssl-enum-ciphers -p 443 intranet.example.com
nmap -sV --script ssh2-enum-algos -p 22 admin01.example.com
```

Nmap also helps with change control. If a ticket says only HTTPS should be exposed, scan the host before and after the change. If the intended result is "SSH closed from this subnet," test from that subnet. The best practice is to turn the desired state into a command that can prove or disprove it.

```bash
# Bad Practice: "The firewall team says it is fixed."

# Best Practice: Validate the expected state from the affected network
nmap -Pn -p 22,443 app01.example.com
```

Output handling matters. Use `-oA` when you want all common output formats with the same basename. Store the results somewhere predictable. Even without a fancy pipeline, this gives you historical evidence when someone asks, "Has this always been open?"

```bash
mkdir -p scans
nmap -sV --top-ports 100 app01.example.com -oA scans/app01-baseline
```

## Solution / Best Practice

Use Nmap as a verification tool with defined scope, not as a random network cannon.

Start with a small operating model:

- Define the authorized networks and hosts.
- Choose a few routine scan profiles.
- Prefer safe discovery, service detection, and approved NSE scripts.
- Save scan output with predictable names.
- Review differences as operational drift, not trivia.

A practical baseline workflow looks like this:

```bash
# 1. Discover live hosts in an authorized range
nmap -sn 192.0.2.0/24 -oA scans/site-discovery

# 2. Check common services on a known server
nmap -sV --top-ports 100 app01.example.com -oA scans/app01-services

# 3. Inspect HTTPS configuration
nmap -sV --script ssl-cert,ssl-enum-ciphers -p 443 app01.example.com -oA scans/app01-tls

# 4. Verify SSH exposure from the current network location
nmap -Pn -p 22 app01.example.com -oA scans/app01-ssh-check
```

For scheduled checks, keep the scope boring and documented. A weekly scan of known servers for expected management ports is useful. A surprise run of every script category against every production host is not administration; it is avoidable noise.

```bash
# Example cron entry for a narrow weekly service inventory
0 3 * * 0 /usr/bin/nmap -sV -p 22,80,443,445 -iL /etc/nmap/managed-hosts.txt -oA /var/log/nmap/managed-services
```

The result should feed a review habit. New service on a server? Find the change. Expired certificate? Fix the renewal path. Unexpected SMB exposure? Check the host role and firewall policy. Nmap does not replace asset management, monitoring, or vulnerability management, but it gives you a fast way to challenge their assumptions.

## Conclusion

Nmap is at its best when system administrators use it to verify the boring things that quietly become outages and findings. Is the host alive? Is the expected service listening? Did the firewall rule work? Is the certificate actually the one you think it is? Are legacy protocols still enabled?

That is not glamorous work, but it is real administration. The network does not care what the spreadsheet says. Use Nmap to check the truth, save the results, and treat drift as something to investigate before it becomes somebody else's incident report.

## Legal Disclaimer
All opinions expressed in this blog post are my own and do not reflect the views of my employer. Technical examples are provided for educational use in systems you own or are authorized to assess.
