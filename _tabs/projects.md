---
layout: page
title: Selected Projects
icon: fas fa-server
order: 5
---

Security tools and infrastructure I have built. Some ran in a lab, some are working tools. Each entry says which, what I did myself, and what is still missing.

---

## Vigie: phishing-link protection before navigation

- **Problem:** A newly created phishing page may not appear in blocklists yet, especially when it targets a bank or mobile money service in West Africa.
- **Who it is for:** People receiving links through WhatsApp, SMS, email or documents, with Togo and West Africa as the first target region.
- **What I did:** Built the investigation server, the web application and the Windows client. Vigie checks redirects, domain signals, page content, brand identity and threat feeds before an AI model reviews the collected evidence. Fixed safety rules make the final decision stricter when needed.
- **Status:** Working web and Windows versions. Android is in progress.
- **Proof:** [Technical write-up and evaluation results](/posts/vigie-link-protection/) · [Live demo](https://vigie.assimti-charles.tech) · [Source code](https://github.com/en2eav0r/vigie)
- **Limit and next step:** The test allowed 57 of 60 legitimate sites. Reducing those false positives and signing the Windows installer come next.

---

## APAST: intrusion detection and alerting

- **Problem:** Small organizations in Togo get attacked and have no detection in place, usually because commercial SIEMs cost too much.
- **Who it is for:** An IT team of one or two people with no security budget.
- **What I did:** Designed the architecture and deployed it: Suricata for the network, Winlogbeat for Windows hosts, Logstash, Elasticsearch and Kibana for storage and dashboards, a Telegram bot for alerts. Supervised by a senior consultant at Cyber Defense Africa.
- **Status:** Lab prototype. Tested with simulated attacks from an isolated attacker network.
- **Proof:** [Architecture, test results and dashboards](/posts/apast-siem-deployment/)
- **Limit and next step:** Detection is rule-based, so it misses anything without a signature, and the default rules are noisy. Next: tune the rules on real traffic and add behavioral detection.

---

## Network IDS lab with SPAN port mirroring

- **Problem:** An IDS needs a copy of network traffic without sitting directly in the data path. A physical TAP would provide passive, non-intrusive visibility, but one was not available for this lab.
- **Who it is for:** Network and security teams that need to monitor traffic with Suricata using existing switching equipment.
- **What I did:** Built the lab on two physical Cisco Catalyst 3560G switches and used SPAN, Cisco's port-mirroring feature, as the alternative to a TAP. Traffic from the monitored interfaces was copied to the switch port connected to the IDS.
- **Status:** Physical lab deployment using real switches.
- **Proof:** [LinkedIn project post](https://lnkd.in/p/eH4dT77D)
- **Limit and next step:** SPAN depends on switch resources and can drop packets when mirrored traffic exceeds the destination port's capacity. A dedicated network TAP would be the better choice for continuous production monitoring.

A TAP, or Test Access Point, observes a critical network link without interrupting or slowing the traffic. It is commonly placed between a switch and a router. When a TAP is not available, SPAN, short for Switched Port Analyzer, can copy traffic from one or more source ports to the interface connected to the IDS. RSPAN extends the same principle across switches.

For example, this Cisco configuration sends a copy of the traffic from ports 1 and 2 to the IDS on port 10:

```text
Switch(config)# monitor session 1 destination interface gigabitethernet0/10
Switch(config)# monitor session 1 source interface gigabitethernet0/1 both
Switch(config)# monitor session 1 source interface gigabitethernet0/2 both
```

Useful references:

- [Deploying Suricata, part 1](https://www.criticaldesign.net/post/how-to-setup-a-suricata-ips)
- [Deploying Suricata with the ELK stack, part 2](https://www.criticaldesign.net/post/how-to-setup-a-suricata-ips-elk-stack)
- [Configuring a SPAN port](https://youtu.be/4XATFxLj7IY)

---

## PCAP Analyzer

- **Problem:** Going through a packet capture by hand in Wireshark is slow, and it's easy to miss a credential or a query buried in a TCP stream.
- **Who it is for:** I developed it for Team AFRICC to use at the International Cybersecurity Challenge 2025 in Japan. It is also useful to analysts doing network forensics and CTF players working on forensics challenges.
- **What I did:** Wrote the whole tool in Python. It rebuilds TCP streams, parses HTTP, Redis and PostgreSQL traffic, uses entropy to spot encrypted payloads, flags credentials, tokens and injection patterns, and exports JSON, text and CSV reports.
- **Status:** Working tool, open source.
- **Proof:** [Source code and documentation](https://github.com/en2eav0r/PCAP-Analyzer)
- **Limit and next step:** Pattern detection is regex-based and produces false positives. There is no automated test suite yet. Next: add tests built on public sample captures.

---

## Active Directory for a client scenario

- **Problem:** A fictional institution, LPRC, needed centralized accounts, policies and updates instead of managing each machine on its own.
- **Who it is for:** The client's IT administrator.
- **What I did:** Led a team of three. We deployed AD DS, DNS, DHCP, group policies, WSUS and an L2TP/IPSec VPN, wrote the deployment manual, priced the hardware and wrote the commercial offer.
- **Status:** Academic lab on VMware Workstation, first year at IPNET Institute.
- **Proof:** [Deployment write-up and the ten acceptance tests signed off by the supervisor](/posts/active-directory-deployment/)
- **Limit and next step:** Every role ran on one server, which is fine for a lab but not for production. I would split them now.

---

## Phishing simulation with Gophish

- **Problem:** A company wants to know how its staff react to a realistic phishing email before a real attacker tests them.
- **Who it is for:** An IT or security team running awareness campaigns.
- **What I did:** Set up Gophish and ran a full campaign: target group, email template, cloned landing page, sending profile, results tracking.
- **Status:** Lab exercise on a fictional company.
- **Proof:** [Step-by-step campaign write-up](/posts/gophish-phishing-simulation/)
- **Limit and next step:** One target and one scenario. A real campaign needs the employer's written consent, a larger group and a training follow-up for people who clicked.
