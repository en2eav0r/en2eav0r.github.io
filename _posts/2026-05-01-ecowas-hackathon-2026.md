---
title: "ECOWAS Regional Hackathon 2026: #1 in Togo across every phase"
date: 2026-05-01 00:00:00 +0000
categories: [Achievements, CTF Competitions]
tags: [ecowas, ctf, jeopardy, koth, boot2root, redteam-tg, togo, west-africa, cybersecurity]
image:
  path: /assets/images/ecowas-2026/ecowas-p1-global-leaderboard.png
  alt: RedTeam-TG — 1st place globally out of 1035 registered participants
---

The ECOWAS Regional Hackathon is not a student event or a local warmup. It is the official cybersecurity competition of the Economic Community of West African States, organized to identify the most technically capable teams across 15 member nations: Benin, Burkina Faso, Cape Verde, Côte d'Ivoire, Gambia, Ghana, Guinea, Guinea-Bissau, Liberia, Mali, Niger, Nigeria, Senegal, Sierra Leone, and Togo. The winner of each country qualifies to represent their nation at the grand final, with all travel, accommodation, and meals covered by ECOWAS. This year, Ghana hosts the final.

We entered as **RedTeam-TG**: en2eav0r, sibisaba, aaron_meta, and ding liren.

The national qualifiers ran from April 1 to May 1, 2026, across three successive elimination phases. We finished first in Togo at every single one.

---

## Phase 1: Jeopardy (April 1 - May 1, 2026)

**1035 registered. 317 teams competing. We placed 1st globally.**

The first phase was a classic Jeopardy CTF, open to all ECOWAS countries simultaneously. 1035 participants registered, forming 317 teams from across West Africa. Every country was competing against every other country on the same leaderboard.

<img src="/assets/images/ecowas-2026/ecowas-registered.png" alt="1035 registered for the ECOWAS Hackathon 2026" style="border-radius: 10px; width: 65%;" />

The challenge set covered 13 categories: Crypto, Discord, Forensics, Misc, Mobile, Network Forensics, OSINT, Pwn, Reverse Engineering, Steganography, and Web. We solved every single challenge in every single category, reaching 100% completion across the board.

<img src="/assets/images/ecowas-2026/ecowas-p1-challenges.png" alt="All categories solved at 100%" style="border-radius: 10px; width: 65%;" />

Final score: **14,150 pts**. On the global leaderboard, covering all 317 teams from every country in West Africa, RedTeam-TG placed **1st**. First place, continent-wide, out of 1,035 registered competitors.

<img src="/assets/images/ecowas-2026/ecowas-p1-global-leaderboard.png" alt="Global leaderboard — RedTeam-TG #1 out of 1035 registered" style="border-radius: 10px; width: 65%;" />

On the Togo-only country leaderboard (49 teams), we also took first place, ahead of Government and FireTeam who finished tied with us on points but behind on solve time.

<img src="/assets/images/ecowas-2026/ecowas-p1-country-top3.png" alt="Country leaderboard — RedTeam-TG #1 in Togo out of 49 teams" style="border-radius: 10px; width: 65%;" />

The top 5 teams per country advance to Phase 2. We went through in first.

<img src="/assets/images/ecowas-2026/ecowas-p1-top5-togo.png" alt="Top 5 qualified for Phase 2 in Togo" style="border-radius: 10px; width: 65%;" />

---

## Phase 2: King of the Hill (April 25, 2026)

**24 participants. 3 Togolese teams. We placed 1st again.**

King of the Hill is a different kind of competition. There are no flags sitting on a server waiting to be retrieved. The objective is to gain root access to a shared machine and write your team name into `/root/team.txt`. As long as your name is in that file, you accumulate points. Every time the machine resets, the race starts over.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth.png" alt="Togo KOTH — Phase 2 overview" style="border-radius: 10px; width: 65%;" />

The machine for this phase was `shambles.local`. Access via Wireguard VPN. The command to claim the machine:

```bash
echo "RedTeam-TG" > /root/team.txt
```

Simple to write, hard to hold. The platform polls every 30 seconds and awards a tick for each interval you control the machine. Resets happen on a fixed schedule, which means every reset is a race between teams to exploit the machine fastest and reclaim root. You need automated tooling: a script ready to fire the moment the reset completes, not a manual workflow.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-details.png" alt="KOTH machine details — target IP and tick structure" style="border-radius: 10px; width: 65%;" />

The other pressure is defensive: once you have the machine, you need to patch the vulnerability you just used. Other teams are running the same exploits. If you leave the entry point open, someone will use it to overwrite your name. You are attacking and defending simultaneously, on the same machine, against live opponents.

We held the machine consistently. Final score for Togo KOTH: RedTeam-TG **2,400 pts**, Government **1,700 pts**, L3arn3rs **1,200 pts**.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-leaderboard.png" alt="Togo KOTH leaderboard — RedTeam-TG #1 with 2400 pts" style="border-radius: 10px; width: 65%;" />

**1st in Togo.** The top 2 teams advance to Phase 3. We qualified in first.

---

## Phase 3: Battleground (April 30, 2026)

**8 participants. 2 Togolese teams. We placed 1st. Only we scored.**

The final phase of the national qualifier was a Boot2Root: one machine, two flags, find `user.txt` then `root.txt` as fast as possible. No time for reconnaissance theatre. This format rewards teams that can move methodically, escalate privileges clean, and not get stuck.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground.png" alt="Togo BattleGround — Phase 3 overview" style="border-radius: 10px; width: 65%;" />

The challenge was called **SeaFish**, worth 500 points. We solved it. The second team, Government, did not score at all.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground-leaderboard.png" alt="BattleGround leaderboard — RedTeam-TG #1 with 500 pts, only team to score" style="border-radius: 10px; width: 65%;" />

Final leaderboard: RedTeam-TG **500 pts**, Government **0 pts**.

**1st in Togo. Again.**

---

## The result

Three phases. Three different formats. Three first-place finishes in Togo. Phase 1: first across all of West Africa, out of 1,035 registered participants.

RedTeam-TG qualified for the ECOWAS Grand Final in Ghana. ECOWAS covers the travel, accommodation, and meals.

I want to be honest about what this took. The jeopardy phase demanded breadth: we had to be competent across cryptography, forensics, web, binary exploitation, steganography, OSINT, and mobile in the same sitting. The KOTH required automation and real-time decision-making, not just exploitation skill. The Boot2Root phase rewarded whoever was fastest and cleanest. Three completely different skill demands, back to back. That is not a format you pass by luck.

Huge respect to sibisaba, aaron_meta, and ding liren. This was a team result at every phase.

To the Togolese teams who pushed hard in Phase 1 (Government, FireTeam, L3arn3rs, Root Access TG): the level was serious. That competition is what made this qualification feel real.

See you in Ghana.
