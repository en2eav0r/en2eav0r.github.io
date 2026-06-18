---
title: "ECOWAS Hackathon 2026 Grand Final: Bronze for Togo in Accra"
date: 2026-06-12 00:00:00 +0000
categories: [Achievements, CTF Competitions]
tags: [ecowas, ctf, jeopardy, koth, code-review, red-vs-blue, battleground, redteam-tg, togo, accra, ghana, west-africa, cybersecurity]
image:
  path: /assets/images/ecowas-final/ecowas-final-cover.jpg
  alt: RedTeam-TG on stage, applauded by the other teams after the ECOWAS Grand Final 2026 results
---

Quick recap before this one: in [the ECOWAS Regional Hackathon 2026 qualifiers](/posts/ecowas-hackathon-2026/), we finished first in Togo across the three phases. That qualified us for the continental Grand Final in Accra. This post is about what happened next.

Writing it down now before the details get fuzzy.

---

## Sunday, June 7: arriving in Accra

Flight to Accra on Sunday, June 7. We landed roughly at the same time as Côte d'Ivoire, Benin, and one Nigerian player (Theory). The hotel had sent someone to the airport to pick us up.

Check-in got delayed but we eventually got our rooms. That evening we went out to grab something at the nearest KFC, half to eat, half to look around the area.

---

## Monday, June 8: briefing

Monday morning, official briefing. Run by the organizers and Mrs. Folake Olagunju, Director of the Digital Economy and Post at the ECOWAS Commission. We got the format, the start time, and the news that took some processing: the scoreboard was going to be hidden for the whole competition. First time I've ever played a CTF without a leaderboard.

We were also told there'd be a separate women's competition in the evening.

We went back to our rooms to plan with the team. With no scoreboard to lean on, every call we made was going to be a guess.

### The full format

| Order | Phase |
|-------|-------|
| 1 | Jeopardy (round 1) |
| 2 | King of the Hill (double session, 4 teams per hill) |
| 3 | Code Review |
| 4 | Red vs Blue (double session) |
| 5 | Battleground (double session) |
| 6 | Jeopardy (final round) |

From what we'd seen in the qualifiers, we already had an idea: KOTH was the round that was going to decide things.

---

## Tuesday, June 9: opening ceremony

Tuesday morning, short ceremony before the actual start. Each country was called to the stage with a track from their own region, and we had to dance to it as the intro. Good way to defuse the tension a bit.

Twelve countries in the room:

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
| 🇹🇬 **Togo** | **RedTeam-TG** |
| 🇸🇱 Sierra Leone | Sudo-SL |
| 🇨🇻 Cape Verde | CyberSharks |
| 🇬🇳 Guinea | SilySec |

<img src="/assets/images/ecowas-final/all-competitors.jpg" alt="Group photo of all 12 competing teams at the ECOWAS Hackathon 2026 Grand Final in Accra" style="border-radius: 10px; width: 65%;" />

---

## Phase 1: Jeopardy

First phase was a wide Jeopardy round.

Most of the challenges were solvable by an LLM. I think the admins expected that. First bloods were dropping everywhere, faster than I'd ever seen in a real CTF. Every team had clearly come prepared, and some had clearly put real work into prepping their agents before flying out.

Not using the same tools as everyone else wasn't really an option. Refusing to play that way just meant losing on principle. So the room basically turned into a race between AI workflows.

We were still solving Jeopardy challenges late on Tuesday night and into Wednesday morning.

---

## Phase 2: King of the Hill

KOTH started Wednesday at 9 a.m. Going in, we knew this was the round where the competition would actually be decided.

If you don't know KOTH: you have to break into a shared machine, get root, then patch the vuln you used so no other team can come in the same way. To prove you have control, you write your team name into `/root/team.txt`. First to claim the hill gets a bonus, every tick your name is still in the file you score, and the machine resets every hour so everyone fights for it again.

<img src="/assets/images/ecowas-final/koth-format.webp" alt="Diagram of the King of the Hill format used at the ECOWAS Grand Final 2026" style="border-radius: 10px; width: 65%;" />

In a competition setting that format is dangerous. Your performance depends entirely on the bracket you draw. If you're in a strong group you have to take the hill first or you bleed points the whole hour. And the gap compounds: once a team is king and patches well, catching up gets really hard.

We landed in the same bracket as Benin. No disrespect to the other teams, Benin was one of the top favorites coming into the final.

The round started. We took the first hill, but our write to `/root/team.txt` wasn't showing on the board. We pinged the admins. No response for a long time. We kept our automation running in the background while we tried to figure out what was actually broken.

> Side note: the admins were not feeding the teams the same info at the same time, which was hard to deal with on the floor.

A few ticks before the hour ended, one of our scripts finally got our name through. Then the next reset hit and the bug was back. We kept waiting, and by the time the round was closing, Benin had taken the hill back and held it to the end.

The second KOTH session, the one that was supposed to give us a chance to recover, got cancelled by the admins because of the issues from the first one.

Meanwhile Nigeria dominated their KOTH bracket from start to finish, and Guinea did the same in theirs. You could already guess who was going to end up at the top.

<img src="/assets/images/ecowas-final/redteam-tg-laptops.jpg" alt="RedTeam-TG focused on their laptops during the ECOWAS Grand Final competition floor" style="border-radius: 10px; width: 65%;" />

---

## Phase 3: Code Review

Next was Code Review. Honestly, this one was a relief. After the AI race in Jeopardy, finally a round that asked teams to actually read code.

Format: an admin projects a piece of C or web code, and the team has to:

1. Point at the vulnerable line,
2. Name the vuln,
3. Explain how to patch it.

Two categories rotated through, even if they weren't announced as such: web and pwn. We had strong pwners and strong web players in the team, so this was our terrain. The "manual side of things," as my teammate meta likes to call it.

A few teams went before us. Among the vulns going around: SQL injection, XSS, buffer overflows, integer overflows, and a few more.

When our turn came, here is an illustrative example in the same spirit as what we got:

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

Again, just an example. Can you find the vuln, name it, and patch it? If yes, that's the kind of reflex the round was rewarding.

### A peek at the standings

Right before Red vs Blue, the admins gave us a rare look at the scoreboard.

- Benin: 2nd
- RedTeam-TG: 3rd
- Guinea: 4th (penalized for starting their KOTH before the official go signal, which also cost them on Code Review)

For us, that was honestly a relief. The fight we put up against Benin in KOTH, despite everything that went wrong, was paying off.

Now we just had to push harder on what was left.

---

## Phase 4: Red vs Blue

Red vs Blue pairs two teams against each other: you attack the opponent's machine and defend yours at the same time. Two rounds. We dropped 100 points on one of them because of a miscommunication with the admins. That one stung.

---

## Phase 5: Battleground

Then Battleground. The room was tense. Word had been going around, unofficially, that there were points for first bloods. Combined with the AI factor from Jeopardy, you couldn't really plan for that. Bloods were dropping fast and from anywhere.

Top 3 still wasn't locked in. Nigeria's team error, on the other hand, had already pulled away from everyone.

First Battleground we drew Guinea's SilySec. They blooded the machine before us. No scoreboard, no way to know how many bloods anyone had on their own machines either. We were basically guessing where we stood.

---

## Final Jeopardy

Last phase was another Jeopardy round, but the scoring had shifted. This round's leaderboard didn't count on its own; only the cumulative total mattered. So the plan was simple and exhausting: solve every flag you can, ignore position, just farm points.

After the round closed, the admins finally pushed the Jeopardy scoreboard out.

There's one part of the day I was genuinely disappointed about. I'm not going to write about it here.

We didn't let up on anything until the countdown hit `00:00:00:00`.

---

## End of the Grand Final

When the timer hit zero, nobody celebrated. Honestly, nobody felt relief either. Just tired, stressed about not knowing, and waiting for the results.

The results ceremony was hard to sit through. Every team got called up, every certificate handed out, and we still didn't know where we'd landed.

Then the host said it: the three remaining teams should stand and go to the back of the room. We stood up. That's when I understood we had actually done it.

<img src="/assets/images/ecowas-final/podium-prizes.jpg" alt="Closing ceremony of the ECOWAS Hackathon 2026 Grand Final with the top teams holding their prizes" style="border-radius: 10px; width: 65%;" />

---

## Final standings — ECOWAS Hackathon 2026 Grand Final

| Rank | Country | Team |
|------|---------|------|
| 🥇 1st | 🇳🇬 Nigeria | error |
| 🥈 2nd | 🇧🇯 Benin | Escadron |
| 🥉 **3rd** | 🇹🇬 **Togo** | **RedTeam-TG** |

<img src="/assets/images/ecowas-final/togo-trophy.jpg" alt="RedTeam-TG with the 3rd place trophy at the ECOWAS Hackathon 2026 Grand Final" style="border-radius: 10px; width: 65%;" />

First time at this stage for me, and a first for Togo at this competition. My teammates and I put Togo on the ECOWAS podium. I still don't really have words for what that felt like.

---

## Closing notes

To RedTeam-TG: this bronze is yours as much as it's mine. Every phase, every dead end, every blind call we had to make without a scoreboard, we made together.

To error (Nigeria) and Escadron (Benin): you were ahead and you earned it. The way you dominated KOTH was hard to argue with.

To Mrs. Folake Olagunju, the ECOWAS Commission, and the Accra organizing team: thanks for hosting twelve countries, taking on the costs, and making this competition exist.

To everyone in Togo who's been following the journey since the qualifiers: this one is for you.

Bronze for Togo, and the first time we've ever been on the ECOWAS Grand Final podium. Still hasn't fully sunk in.

<style>
img.emoji {
  height: 1.1em;
  width: auto;
  margin: 0 .1em;
  vertical-align: -0.2em;
  display: inline-block;
}
</style>
<script src="https://cdn.jsdelivr.net/npm/@twemoji/api@latest/dist/twemoji.min.js" crossorigin="anonymous"></script>
<script>
  if (window.twemoji) {
    twemoji.parse(document.body, { folder: 'svg', ext: '.svg' });
  }
</script>
