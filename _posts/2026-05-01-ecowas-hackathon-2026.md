---
title: "ECOWAS Regional Hackathon 2026: #1 in Togo across every phase"
date: 2026-05-01 00:00:00 +0000
categories: [Achievements, CTF Competitions]
tags: [ecowas, ctf, jeopardy, koth, boot2root, redteam-tg, togo, west-africa, cybersecurity]
image:
  path: /assets/images/ecowas-2026/ecowas-cover.png
  alt: Official logo of the 2026 ECOWAS Regional Hackathon
---

The ECOWAS Regional Hackathon is the official cybersecurity competition of the Economic Community of West African States, built around one goal: finding the strongest teams across 15 member nations. Benin, Burkina Faso, Cape Verde, Côte d'Ivoire, Gambia, Ghana, Guinea, Guinea-Bissau, Liberia, Mali, Niger, Nigeria, Senegal, Sierra Leone, and Togo each run their own national qualifier. The team that finishes first in their country earns a spot at the continental grand final, with all travel, accommodation, and meals covered by ECOWAS. The 2026 final is in Ghana.

I competed as a member of **RedTeam-TG**.

The national qualifiers ran from April 1 to May 1, 2026, across three elimination phases. We finished first in Togo at every single one.

---

## Phase 1: Jeopardy (April 1 - May 1, 2026)

**1,035 registered participants. 317 teams competing. We placed 1st across all of West Africa.**

The first phase was a Jeopardy-style CTF, open to all ECOWAS countries at the same time on a shared global leaderboard. 1,035 participants signed up, split into 317 teams. Every country competed against every other country. There was no separate bracket for Togo — we were ranked alongside every team on the continent from the start.

<img src="/assets/images/ecowas-2026/ecowas-registered.png" alt="The ECOWAS Hackathon 2026 platform showing 1,035 registered participants across all member states" style="border-radius: 10px; width: 65%;" />

The challenge set spanned 13 categories: Crypto, Discord, Forensics, Misc, Mobile, Network Forensics, OSINT, Pwn, Reverse Engineering, Steganography, and Web. We solved all of them.

<img src="/assets/images/ecowas-2026/ecowas-p1-challenges.png" alt="Challenge board showing all 13 categories completed at 100%" style="border-radius: 10px; width: 65%;" />

Final score: **14,150 pts**. On the global leaderboard, RedTeam-TG placed **1st** — ahead of all 317 teams from every ECOWAS country, out of 1,035 registered competitors.

<img src="/assets/images/ecowas-2026/ecowas-p1-global-leaderboard.png" alt="Global leaderboard for Phase 1: RedTeam-TG ranked 1st out of 317 teams and 1,035 registered participants across West Africa" style="border-radius: 10px; width: 65%;" />

On the Togo country leaderboard (49 teams), we also took first place. Government and FireTeam tied on points but finished behind us on solve time.

<img src="/assets/images/ecowas-2026/ecowas-p1-country-top3.png" alt="Togo country leaderboard for Phase 1: RedTeam-TG in 1st place out of 49 Togolese teams" style="border-radius: 10px; width: 65%;" />

The top 5 teams per country advance to Phase 2. We went through in first.

<img src="/assets/images/ecowas-2026/ecowas-p1-top5-togo.png" alt="Top 5 Togolese teams qualified for Phase 2, with RedTeam-TG leading the standings" style="border-radius: 10px; width: 65%;" />

---

## Phase 2: King of the Hill (April 25, 2026)

**24 participants. 6 teams. We placed 1st again.**

King of the Hill works differently from a regular CTF. There are no flags to find and submit. Instead, you gain root access to a shared machine and write your team name into `/root/team.txt`. Every 30 seconds the platform polls the file and awards a point to whoever controls it at that moment. When the machine resets, the race starts over from scratch.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth.png" alt="Togo KOTH challenge page showing the King of the Hill format for Phase 2, April 25, 2026" style="border-radius: 10px; width: 65%;" />

The target machine for this phase was `shambles.local`, connected via Wireguard VPN. Claiming it meant running:

```bash
echo "RedTeam-TG" > /root/team.txt
```

Simple command. Hard to keep. Each reset is a race — you need to re-exploit the machine and reclaim root before anyone else does. That requires pre-written, automated tooling. A manual approach loses too much time.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-details.png" alt="KOTH instance details: target IP 10.100.10.46, team.txt path, and tick scoring structure" style="border-radius: 10px; width: 65%;" />

There is a defensive layer too. Once you have the machine, you need to patch the vulnerability you just exploited — because every other team is running the same attacks. If you leave the entry point open, someone writes over your name and your tick goes to them. You are attacker and defender at the same time, on the same machine, against live opponents.

We held control consistently throughout. Final score for Togo KOTH: RedTeam-TG **2,400 pts**, Government **1,700 pts**, L3arn3rs **1,200 pts**.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-leaderboard.png" alt="Togo KOTH country leaderboard: RedTeam-TG in 1st place with 2,400 points, ahead of Government at 1,700 and L3arn3rs at 1,200" style="border-radius: 10px; width: 65%;" />

**1st in Togo.** The top 2 teams advance to Phase 3. We qualified in first.

---

## Phase 3: Battleground (April 30, 2026)

**8 participants. 2 Togolese teams. We placed 1st. We were the only team to score.**

The final phase was a Boot2Root: one machine, two flags, find `user.txt` then `root.txt` as fast as possible. Speed and precision both matter. You cannot afford to get stuck, and you cannot afford to rush into rabbit holes.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground.png" alt="Togo BattleGround challenge page showing the Boot2Root format for Phase 3, April 30, 2026" style="border-radius: 10px; width: 65%;" />

The machine was named **SeaFish**, worth 500 points. We solved it. The other qualified team, Government, did not score.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground-leaderboard.png" alt="Togo BattleGround final leaderboard: RedTeam-TG in 1st with 500 points, the only team to complete the Boot2Root challenge" style="border-radius: 10px; width: 65%;" />

Final leaderboard: RedTeam-TG **500 pts**, Government **0 pts**.

**1st in Togo. Again.**

---

## The result

Three phases, three formats, three first-place finishes in Togo. And in Phase 1, first place across the entire continent — out of 1,035 registered participants from every ECOWAS country.

RedTeam-TG qualified for the ECOWAS Grand Final in Ghana. ECOWAS covers the travel, accommodation, and meals for the national representatives.

I want to be honest about what this required. The Jeopardy phase tested breadth: you had to hold your own in cryptography, web exploitation, forensics, binary exploitation, steganography, OSINT, and mobile security all at once. The KOTH tested something else entirely — automation, speed, and the ability to switch between attacking and defending mid-session. The Boot2Root tested pure methodology under pressure. None of these formats overlap much. Doing well in all three in a single qualifier is not a matter of knowing one thing deeply, it is a matter of knowing many things well enough to apply them fast.

Huge respect to all of the team members. This was a team result at every phase.

To the Togolese teams who pushed in Phase 1 (Government, FireTeam, L3arn3rs, Root Access TG): the competition was real. That pressure is part of what made this qualification meaningful.

See you in Ghana.
