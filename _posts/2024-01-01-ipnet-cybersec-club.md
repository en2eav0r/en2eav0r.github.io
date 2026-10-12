---
title: "IPNET CyberSec Club: Building the Next Generation of Cybersecurity Professionals"
date: 2024-01-01 00:00:00 +0000
categories: [Achievements, Academic]
tags: [club, cybersecurity, ipnet, training, togo, leadership, discord, bot]
---

> *Founded and led by ASSIMTI Essowèréou Charles: President & Head of Infrastructure, IPNET Institute of Technology, Togo.*

## What I built

I founded and currently lead the **IPNET CyberSec Club**, a structured cybersecurity training program that grew from zero to **60+ active members** spanning every academic level (L1 to M2). It's not a hobby club. It's a full pedagogical system I designed, built, and operate.

To run it at scale, I took the open-source **[cyberstudent-ctf](https://github.com/cyberstudent-ctf)** bot as a foundation and extended it significantly, adding a mandatory writeup system, tiered group management, anti-cheat enforcement, milestone role progression, and admin tracking.

**Tech stack:**

| Layer | Technology | Why |
|-------|-----------|-----|
| Bot Framework | Sapphire Framework + Discord.js v14 | Event-driven, modular, production-grade |
| Database | PostgreSQL + Prisma ORM | Persistent tracking, relational data integrity |
| Language | TypeScript | Type-safety across a complex codebase |
| Deployment | Docker + PM2 | 24/7 uptime, zero-downtime restarts |
| Validation | Zod | Input integrity and security |

---

## Impact at a glance

| Metric | Result |
|--------|--------|
| Active members trained | **60+** (L1 to M2) |
| Weekly training sessions | Every Saturday, year-round |
| National competition, teams present | **7 out of 10** trained by this club |
| National ANCY CTF | **1st place** |
| International representation | **2 members** selected for **ICC Japan** |

**Seven of the ten teams at the national cybersecurity competition included people trained by the club.**

These are program-level results, not individual claims. My role was to found the club, design its training model, lead its operation and extend the technical infrastructure used to manage participation.

---

## What makes this different

### Mandatory writeup system
No points without a writeup: a technical report covering the approach, tools used, exploitation steps, and remediation. Every member builds a security portfolio from day one.

### Three skill tiers
Members are split into `I-Am-New`, `I-ve-One-Year-Experience`, and `I-ve-More-Experience`. Each group gets a calibrated challenge every Saturday. Beginners are challenged without being crushed.

### "Learn by teaching" requirement
Anyone who solves a challenge has to present their method to the group. It develops the kind of technical clarity that matters in interviews and real work, not just CTF scores.

### How the culture runs
- **Zero-tolerance deadline policy**: writeup by the following Saturday or temporary exclusion
- **Anti-cheat system**: the bot monitors all channels and removes any flag shared outside authorized areas
- **Best Cyber Battle Woman award**: a deliberate choice to recognize women in cybersecurity
- **Togo's first university-run CTF**: the [IPNET Cyber Battle](/posts/ipnet-cyber-battle/)

---

## Results

- **ICC Japan**: 2 club members selected to represent Togo
- **ANCY National CTF**: 1st place
- **National competition**: 7 of the 10 participating teams had been trained through the club

## From training to public competition

The club's weekly cycle created the participant base, but students also needed a real event in which to test themselves. That led to the [IPNET Cyber Battle](/posts/ipnet-cyber-battle/), a university-organized CTF built by students and opened as a visible showcase of their skills.

The competitive results are documented separately:

- [Togo National CTF 2024: 1st place](/posts/togo-national-ctf/)
- [Togo National CTF 2024: technical writeup](/posts/wu-ctf-national-togo/)
- [ICC Japan 2025: two Togolese representatives selected from the club](/posts/icc-japan/)

## What still needs work

The system is effective, but it is demanding to operate. Weekly challenge preparation, writeup review and member tracking depend on sustained volunteer effort. The next stage is to document more of the curriculum, improve mentor delegation and make progress data easier to review without weakening the anti-cheat rules.
