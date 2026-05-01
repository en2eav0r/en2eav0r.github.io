---
title: "ECOWAS Regional Hackathon 2026: #1 in Togo across every phase"
date: 2026-05-01 00:00:00 +0000
categories: [Achievements, CTF Competitions]
tags: [ecowas, ctf, jeopardy, koth, boot2root, redteam-tg, togo, west-africa, cybersecurity]
image:
  path: /assets/images/ecowas-2026/ecowas-cover.png
  alt: Official logo of the 2026 ECOWAS Regional Hackathon
---

The ECOWAS Regional Hackathon is the official cybersecurity competition of the Economic Community of West African States. Each of the 15 member nations runs a national qualifier (Benin, Burkina Faso, Cape Verde, Côte d'Ivoire, Gambia, Ghana, Guinea, Guinea-Bissau, Liberia, Mali, Niger, Nigeria, Senegal, Sierra Leone, and Togo), and the team that finishes first in their country qualifies for the continental grand final. ECOWAS covers the travel costs, accommodation, and meals. The 2026 final is in Ghana.

I competed as a member of **RedTeam-TG**.

The national qualifiers ran from April 1 to May 1, 2026, across three successive elimination phases. We finished first in Togo at every one.

---

## Phase 1: Jeopardy (April 1 - May 1, 2026)

**1,035 registered participants. 317 teams. We placed 1st across all of West Africa.**

The first phase was a Jeopardy-style CTF on a shared global leaderboard, open to every ECOWAS country at the same time. 1,035 participants registered and formed 317 teams. Every country competed on the same board. Togo was not in a separate bracket; we were ranked alongside every team from every nation from day one.

<img src="/assets/images/ecowas-2026/ecowas-registered.png" alt="The ECOWAS Hackathon 2026 platform showing 1,035 registered participants across all member states" style="border-radius: 10px; width: 65%;" />

The challenge set covered 13 categories: Crypto, Discord, Forensics, Misc, Mobile, Network Forensics, OSINT, Pwn, Reverse Engineering, Steganography, and Web. We solved everything.

<img src="/assets/images/ecowas-2026/ecowas-p1-challenges.png" alt="Challenge board showing all 13 categories completed at 100%" style="border-radius: 10px; width: 65%;" />

Final score: **14,150 pts**. RedTeam-TG placed **1st** on the global leaderboard, ahead of all 317 teams across West Africa.

<img src="/assets/images/ecowas-2026/ecowas-p1-global-leaderboard.png" alt="Global leaderboard for Phase 1: RedTeam-TG ranked 1st out of 317 teams and 1,035 registered participants across West Africa" style="border-radius: 10px; width: 65%;" />

On the Togo country leaderboard (49 teams), we also took first. Government and FireTeam tied us on points but finished behind on solve time.

<img src="/assets/images/ecowas-2026/ecowas-p1-country-top3.png" alt="Togo country leaderboard for Phase 1: RedTeam-TG in 1st place out of 49 Togolese teams" style="border-radius: 10px; width: 65%;" />

The top 5 per country advance to Phase 2. We went through in first.

<img src="/assets/images/ecowas-2026/ecowas-p1-top5-togo.png" alt="Top 5 Togolese teams qualified for Phase 2, with RedTeam-TG leading the standings" style="border-radius: 10px; width: 65%;" />

---

## Phase 2: King of the Hill (April 25, 2026)

**24 participants. 6 teams. We placed 1st again.**

In King of the Hill, there are no flags to retrieve. You gain root access to a shared machine and write your team name into `/root/team.txt`. Every 30 seconds the platform checks the file and awards a point to whoever holds it. When the machine resets, the fight starts over.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth.png" alt="Togo KOTH challenge page showing the King of the Hill format for Phase 2, April 25, 2026" style="border-radius: 10px; width: 65%;" />

The machine for this phase was `shambles.local`, accessed via Wireguard VPN. Claiming it:

```bash
echo "RedTeam-TG" > /root/team.txt
```

One line. But keeping your name in that file across every reset is what the competition actually measures. Each reset triggers a race: whoever re-exploits the machine and writes their name first takes the tick. You cannot do that manually at competitive speed. You need scripts ready to fire the instant the machine comes back up.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-details.png" alt="KOTH instance details: target IP 10.100.10.46, team.txt path, and tick scoring structure" style="border-radius: 10px; width: 65%;" />

On top of attacking, you have to defend. Once you own the machine, you patch the vulnerability you just used, because every other team is trying the same entry point. Leave it open and someone overwrites your name. You are attacking and defending simultaneously, against live opponents, on a machine that resets every hour.

We held control consistently. Final scores: RedTeam-TG **2,400 pts**, Government **1,700 pts**, L3arn3rs **1,200 pts**.

<img src="/assets/images/ecowas-2026/ecowas-p2-koth-leaderboard.png" alt="Togo KOTH country leaderboard: RedTeam-TG in 1st place with 2,400 points, ahead of Government at 1,700 and L3arn3rs at 1,200" style="border-radius: 10px; width: 65%;" />

**1st in Togo.** The top 2 teams advance to Phase 3. We went through in first.

---

## Phase 3: Battleground (April 30, 2026)

**8 participants. 2 Togolese teams. We placed 1st. We were the only team to score.**

The final phase was a Boot2Root: one machine, two flags, find `user.txt` then `root.txt` as fast as possible. The format is unforgiving. Going too slow lets the other team finish first, but chasing the wrong path costs time you cannot get back.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground.png" alt="Togo BattleGround challenge page showing the Boot2Root format for Phase 3, April 30, 2026" style="border-radius: 10px; width: 65%;" />

The machine was named **SeaFish**, worth 500 points. We solved it. The other qualified team, Government, did not score.

<img src="/assets/images/ecowas-2026/ecowas-p3-battleground-leaderboard.png" alt="Togo BattleGround final leaderboard: RedTeam-TG in 1st with 500 points, the only team to complete the Boot2Root challenge" style="border-radius: 10px; width: 65%;" />

Final leaderboard: RedTeam-TG **500 pts**, Government **0 pts**.

**1st in Togo. Again.**

---

## The result

We finished first in Togo at every phase of the national qualifier. In Phase 1 we also placed first across the entire continent, out of 1,035 registered participants from every ECOWAS country.

RedTeam-TG is qualified for the ECOWAS Grand Final in Ghana.

I will be honest about what that took. The Jeopardy phase required breadth: you had to perform across cryptography, web exploitation, forensics, binary exploitation, steganography, OSINT, and mobile security in a single session. The KOTH was a completely different kind of pressure: automation, reaction speed, and the ability to flip between offense and defense without losing your footing. The Boot2Root rewarded clean methodology and composure under time pressure. These three skill sets do not overlap much. Getting through all three phases in first place is not a coincidence.

Huge respect to all the team members. This was a collective result at every stage.

To the Togolese teams who competed hard in Phase 1 (Government, FireTeam, L3arn3rs, Root Access TG): the level was high and that made the qualifier worth winning.

See you in Ghana.
