---
layout: page
title: Selected Projects
icon: fas fa-server
order: 5
---

Security tools and infrastructure I have built. Some ran in a lab, some are working tools. Each entry says which, what I did myself, and what is still missing.

---

## APAST: intrusion detection and alerting

- **Problem:** Small organizations in Togo get attacked and have no detection in place, usually because commercial SIEMs cost too much.
- **Who it is for:** An IT team of one or two people with no security budget.
- **What I did:** Designed the architecture and deployed it: Suricata for the network, Winlogbeat for Windows hosts, Logstash, Elasticsearch and Kibana for storage and dashboards, a Telegram bot for alerts. Supervised by a senior consultant at Cyber Defense Africa.
- **Status:** Lab prototype. Tested with simulated attacks from an isolated attacker network.
- **Proof:** [Architecture, test results and dashboards](/posts/apast-siem-deployment/)
- **Limit and next step:** Detection is rule-based, so it misses anything without a signature, and the default rules are noisy. Next: tune the rules on real traffic and add behavioral detection.

---

## PCAP Analyzer

- **Problem:** Going through a packet capture by hand in Wireshark is slow, and it's easy to miss a credential or a query buried in a TCP stream.
- **Who it is for:** Analysts doing network forensics, and CTF players working on forensics challenges.
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
