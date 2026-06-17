---
title: "From Lomé to Accra: Togo's First Podium at the ECOWAS Hackathon Grand Final"
date: 2026-06-12 00:00:00 +0000
categories: [Achievements, CTF Competitions]
tags: [ecowas, ctf, jeopardy, koth, code-review, red-vs-blue, battleground, redteam-tg, togo, accra, ghana, west-africa, cybersecurity]
image:
  path: /assets/images/ecowas-final/ecowas-final-cover.jpg
  alt: RedTeam-TG on stage, applauded by the other teams after the ECOWAS Grand Final 2026 results
---

A few weeks before this post, I wrote about the [ECOWAS Regional Hackathon 2026 national qualifiers](/posts/ecowas-hackathon-2026/), where we finished first in Togo across all three phases and earned our ticket to the continental Grand Final. This one is about what happened next: Accra, four days, twelve countries, and a podium I will remember for a long time.

This post is more personal than the qualifier one. The format was new, the pressure was different, and so much happened in so little time that I wanted to write it down before it blurred.

---

## Sunday, June 7 — Flying into Accra

We took our flight to Accra on Sunday, June 7, 2026. We landed alongside the Ivorian team, the Beninese team, and one member of the Nigerian team (Theory). A hotel representative was waiting at the airport to handle our transfer.

There was a delay at check-in, but we eventually got our rooms. In the evening, we walked out to grab something to eat at the local KFC, partly to refuel and partly to get a first look at the area.

---

## Monday, June 8 — Briefing and one frustrating reveal

Monday morning was the official competition briefing, run by the organizers and **Mrs. Folake Olagunju**, Director of the Digital Economy and Post at the ECOWAS Commission. She walked us through the format, the start time, and one piece of news that hit hard: **the scoreboard would be hidden for the entire competition**. A first in every CTF I have ever competed in.

We were also told there would be a separate competition dedicated to women, running in the evening.

We pulled back to our rooms to plan with the team. With no scoreboard to lean on, every decision was going to feel blind.

### The full format

| Order | Phase |
|-------|-------|
| 1 | Jeopardy (round 1) |
| 2 | King of the Hill (double session, 4 teams per hill) |
| 3 | Code Review |
| 4 | Red vs Blue (double session) |
| 5 | Battleground (double session) |
| 6 | Jeopardy (final round) |

Looking at the qualifiers, we already knew the killer phase would be **KOTH**.

---

## Tuesday, June 9 — Opening ceremony

Tuesday morning opened with a short ceremony before the competition kicked off. Each country was called to the stage, introduced with a track from their own region, and expected to dance to it. A nice way to take the temperature down before the real fight began.

**Twelve countries** were in the room:

| Country | Team |
|---------|------|
| 🇬🇭 Ghana | CYCLONE |
| 🇬🇲 Gambia | GamHunters |
| 🇨🇮 Côte d'Ivoire | Back2Root |
| 🇸🇳 Senegal | Jambars |
| 🇧🇯 Benin | Escadron |
| 🇱🇷 Liberia | Lyberia Cybers Warriors |
| 🇬🇼 Guinea-Bissau | Cyber_gw |
| 🇳🇬 Nigeria | error |
| 🇹🇬 Togo | **RedTeam-TG** |
| 🇸🇱 Sierra Leone | Sudo-SL |
| 🇨🇻 Cape Verde | CyberSharks |
| 🇬🇳 Guinea | SilySec |

<img src="/assets/images/ecowas-final/all-competitors.jpg" alt="Group photo of all 12 competing teams at the ECOWAS Hackathon 2026 Grand Final in Accra" style="border-radius: 10px; width: 65%;" />

---

## Phase 1: Jeopardy — the AI war

The first phase was a wide Jeopardy round.

The honest truth: most of the challenges were trivially solvable with an LLM. I think the admins knew. First bloods were going off everywhere, faster than I had ever seen in a real CTF. Every team had clearly come armed for this. You could feel that some teams had spent serious time tuning their agents.

The problem is that you cannot opt out of using the tools the others are using. Opting out is just losing slower. So the room turned into an AI race where the edge went to whoever had built the cleaner workflow before getting on the plane.

We were still grinding through Jeopardy challenges on Tuesday night and into the next morning.

---

## Phase 2: King of the Hill — the nerve center

KOTH was scheduled for Wednesday at 9 a.m., and we all knew this was the point everything would pivot around.

For anyone unfamiliar: in King of the Hill, your job is to exploit a vulnerable machine, take root, and **then patch the vulnerability you just used** so no other team can come in through the same door. Proof of control is writing your team name to `/root/team.txt`. From there:

- You get points for being the first to claim the hill,
- And you get points on every tick you are still the team in that file.
- The machine resets every hour, and the fight restarts from zero.

<img src="/assets/images/ecowas-final/koth-format.webp" alt="Diagram of the King of the Hill format used at the ECOWAS Grand Final 2026" style="border-radius: 10px; width: 65%;" />

In a competition context, this is brutal for two reasons:

1. **Your outcome depends entirely on the bracket you draw.** If you are in a strong group, you have to take the hill first or you are bleeding ticks the whole hour.
2. **The gap snowballs.** Once a team is king, every tick they hold pushes them further out of reach.

We drew the bracket with **Benin**. No disrespect to anyone else, Benin was one of the heaviest favorites going in.

The round started, we managed to take the first hill, but our write to `/root/team.txt` was not showing up on the board. We flagged the admins. We waited. For a long time we got nothing back, so we kept our automation scripts running in the background while we tried to understand what was breaking.

> Side note: the admins were not giving every team the same information at the same time, which was hard to deal with in the moment.

A few ticks before the end of the hour, one of our automations finally got our name through. Then the next reset hit and the same issue came back. We kept waiting, and by the time the round was wrapping up, **Benin had retaken the hill and held it to the end**.

The second KOTH session, the one that was supposed to let us recover, was **cancelled** by the admins because of the problems in the first one.

Meanwhile, **Nigeria dominated their KOTH bracket from start to finish**, and Guinea was doing the same in theirs. The shape of the top of the leaderboard was already visible.

<img src="/assets/images/ecowas-final/redteam-tg-laptops.jpg" alt="RedTeam-TG focused on their laptops during the ECOWAS Grand Final competition floor" style="border-radius: 10px; width: 65%;" />

---

## Phase 3: Code Review — back to fundamentals

Next came **Code Review**, and honestly, this one was a relief. After watching the AI war in Jeopardy, finally a phase that asked teams to actually read code.

Format: an admin projects a piece of C or web code, and the team has to:

1. Point at the vulnerable line,
2. Name the vulnerability,
3. Explain how to patch it.

Two undeclared categories rotated through: **web** and **pwn**. In the team we had solid pwners and solid web players, so this was our terrain. Manual analysis, the way my teammate **meta** likes to put it.

A handful of teams went before us. The vulns going around included SQL injection, XSS, buffer overflows, integer overflows, and more.

When our turn came, the code looked roughly like this:

```c
#include <stdio.h>

int main() {
    char input[256];
    printf("Entrez votre nom : ");
    fgets(input, sizeof(input), stdin);
    printf(input);
    return 0;
}
```

I will leave it as an exercise: can you spot the vulnerability, name it, and patch it? If yes, you would have scored on that round. If not, you would have lost it.

### A peek at the standings

Right before the Red vs Blue session, the admins gave us a rare look at the scoreboard.

- **Benin: 2nd**
- **RedTeam-TG: 3rd**
- Guinea: 4th (penalized for starting their KOTH before the official go signal, which also cost them ground on Code Review)

For us, that was a real relief. The fight we put up against Benin in KOTH, even with everything that went wrong, was paying off on the board.

Now we had to push harder on the remaining phases.

---

## Phase 4: Red vs Blue

Red vs Blue pairs two teams against each other: each side attacks the opponent's machine while defending their own. We played two rounds. We **lost 100 points** on one of them because of a miscommunication with the admins. That one stung.

---

## Phase 5: Battleground

After that came Battleground. The mood in the room was tense. We had heard, **unofficially**, that there would be points for first bloods. Combined with the AI factor still in play from Jeopardy, first bloods were not something you could plan around. They were happening fast and from anywhere.

Top 3 was still not locked in. **Nigeria's team error**, on the other hand, had already put themselves out of reach.

In our first Battleground we faced Guinea's **SilySec**. They blooded the machine before us. With no visible scoreboard and no way to know how many bloods anyone else had on their own machines, we were left to guess where we stood.

---

## Final Jeopardy — chase every flag

The last phase was another Jeopardy round, but the rules had shifted. The scoreboard for this round did not count standalone, only the cumulative point total did. So the strategy was simple and exhausting: **solve every flag you can, ignore positioning, focus on points.**

After the round closed, the admins finally pushed the Jeopardy scoreboard out.

There is one part of the day I was genuinely disappointed about and will not write about here.

We refused to let go of anything until the countdown hit `00:00:00:00`.

---

## End of the Grand Final

When time was called, there was no relief. No celebration. Just uncertainty, fatigue, a low hum of anxiety, and the wait.

The results announcement was hard to sit through. Every team had been called up, every certificate handed out, and we still did not know.

Then the host said it: **the three remaining teams should stand and walk to the back of the room**. We stood up. That is when I understood we had done it.

<img src="/assets/images/ecowas-final/podium-prizes.jpg" alt="Closing ceremony of the ECOWAS Hackathon 2026 Grand Final with the top teams holding their prizes" style="border-radius: 10px; width: 65%;" />

---

## Final standings — ECOWAS Hackathon 2026 Grand Final

| Rank | Country | Team |
|------|---------|------|
| 🥇 1st | 🇳🇬 Nigeria | error |
| 🥈 2nd | 🇧🇯 Benin | Escadron |
| 🥉 **3rd** | 🇹🇬 **Togo** | **RedTeam-TG** |

<img src="/assets/images/ecowas-final/togo-trophy.jpg" alt="RedTeam-TG with the 3rd place trophy at the ECOWAS Hackathon 2026 Grand Final" style="border-radius: 10px; width: 65%;" />

For my first time at this stage, and a first for Togo at this competition, my teammates and I put **Togo on the ECOWAS podium**. I still do not have a word for what that felt like.

---

## Closing notes

A few things I want to put on the record.

To **RedTeam-TG**: every phase of this final, every dead-end, every workaround, every blind decision we made without a scoreboard, we made together. The bronze is yours as much as it is mine.

To **error (Nigeria)** and **Escadron (Benin)**: you were ahead and you earned it. Hard to argue with KOTH dominance like that.

To **Mrs. Folake Olagunju** and the ECOWAS Commission, and to the organizers in Accra: thank you for hosting twelve countries under one roof, paying for it, and making this competition exist.

To everyone in Togo who has been following the journey since the qualifiers: this one is for you.

**Accra, June 2026. Bronze. First time on the ECOWAS Grand Final podium for Togo. We are not stopping here.**
