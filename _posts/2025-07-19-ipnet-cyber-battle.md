---
title: "IPNET Cyber Battle: Togo's First University-Organized CTF"
date: 2025-07-19 00:00:00 +0000
categories: [Achievements, Academic]
tags: [ctf, cybersecurity, togo, ipnet, organization, first]
image:
  path: /assets/images/cyberbattle-group.jpg
  alt: IPNET Cyber Battle 2025 participants
---

> *"Where Cyber Warriors Rise": organized entirely by students.*

## Watch the event

[<img src="https://img.youtube.com/vi/HdqJ9TKKMVw/maxresdefault.jpg" alt="IPNET Cyber Battle: Official Video" style="border-radius: 10px; width: 65%;" />](https://www.youtube.com/watch?v=HdqJ9TKKMVw&t=1s)

---

## Overview

The **IPNET Cyber Battle** is the annual CTF organized by the IPNET CyberSec Club at IPNET Institute of Technology, Togo.

Before this event, no university in Togo had ever organized a cybersecurity competition. The IPNET CyberSec Club built one: designed the challenges, deployed the infrastructure, ran the event from start to finish.

---

## The team

<img src="/assets/images/cyberbattle-group.jpg" alt="Group photo: IPNET Cyber Battle participants" style="border-radius: 10px; width: 65%;" />

---

## Inside the competition

<img src="/assets/images/cyberbattle-supervision.jpg" alt="Charles supervising a challenger" style="border-radius: 10px; width: 65%;" />

*ASSIMTI Essowèréou Charles: President of the club, on the floor during the competition, supervising challengers in real time.*

<img src="/assets/images/cyberbattle-girls-collab.jpg" alt="Two participants collaborating" style="border-radius: 10px; width: 65%;" />

---

## Competition format

### Phase 1: pre-qualification (OSINT + Forensics + Steganography)

Candidates had to clear a **10-step puzzle chain** before reaching the main CTF:

| Step | Action | Skill tested |
|------|--------|-------------|
| 1 | Analyze a LinkedIn video containing **7 hidden QR codes** | OSINT · Visual analysis |
| 2 | Decode a **Python ASCII script** from a QR code → hidden message | Scripting · Encoding |
| 3 | Find a **QR code hidden at the end of a YouTube video** | OSINT · Attention to detail |
| 4 | Retrieve a PDF from Proton Drive, containing a **hidden text message** revealed via Word | Steganography · Document forensics |
| 5 | Search LinkedIn "À propos" for a **reversed encoded string** → decode with CyberChef | OSINT · CyberChef |
| 6 | Download `message.jpg`, fix **corrupted magic bytes** to reveal the image | File forensics · Hex editing |
| 7 | Run `steghide extract` on the corrected image → extract `secret.txt` | Steganography |
| 8 | Decode **multiple layers of Base64 + Reverse** in `secret.txt` → get ZIP password | Cryptography |
| 9 | Open `Graal.zip` → find **6 unique images** (duplicates filtered with a bash script) | Scripting |
| 10 | Assemble characters from each image → construct the final flag | Image forensics |

**Final flag:**
```
IPNET{y0u_4r3_th3_b3st!bRuh_3ssow7r3oU_g1v3_y0u_4cc3s_t0_th3_r0und_t4bl3_0f_fin4l1sT!!_65547926$$64}
```

> `3ssow7r3oU` in the flag is a direct reference to the creator: ASSIMTI **Essowèréou** Charles. A signature hidden in plain sight.

---

### Phase 2: main CTF (Jeopardy format)

| Aspect | Details |
|--------|---------|
| Format | Jeopardy CTF |
| Categories | Web, Cryptography, Forensics, OSINT, Reverse Engineering, Steganography, Pwn |
| Platform | CTFd |
| Organization | Entirely student-run |

---

## Scoreboard

<img src="/assets/images/cyberbattle-scoreboard.png" alt="Scoreboard IPNET Cyber Battle 2025" style="border-radius: 10px; width: 65%;" />

| Rank | Pseudo | Real Name | Score |
|------|--------|-----------|-------|
| 1 | Arlesh | Abdoulaye Abiboulaye | 4 050 pts |
| 2 | Gr3G | Bakonda Wenssa Grégoire | 3 100 pts |
| 3 | SAY | Ayao Mawonyé Jean-Marie | 2 700 pts |
| 4 | Abdól | Sitou Abdel Maaliki | 2 100 pts |
| 5 | t0g37h3R | — | 2 000 pts |
| 6 | Indice20 | — | 1 600 pts |
| 7 | F_$oCieTy | — | 1 200 pts |
| 8 | Ami1r | — | 1 200 pts |
| 9 | Dmod | — | 1 050 pts |
| 10 | F34r_DrK | — | 1 050 pts |

<img src="/assets/images/cyberbattle-winner.jpg" alt="Official winners: IPNET Cyber Battle 2025" style="border-radius: 10px; width: 65%;" />

*13 teams competed. These three stood on top.*

---

## Awards

Beyond the scoreboard, four special awards:

| Prize | Description |
|-------|-------------|
| Top 3 Teams | Overall performance ranking |
| Blood Hunter | First blood on the hardest challenge |
| Best Writeup | Top quality technical documentation |
| Best Cyber Battle Woman | Top female participant |

### Blood Hunter: Sitou Abdel Maaliki

<img src="/assets/images/cyberbattle-bloodhunter.jpg" alt="Blood Hunter award: Sitou Abdel Maaliki" style="border-radius: 10px; width: 65%;" />

The **Blood Hunter** award goes to whoever draws first blood on the hardest challenge. **Sitou Abdel Maaliki** (`Abdól`), ranked 4th overall, took it. 4th on the scoreboard but first on the challenge that stopped everyone else.

### Best Cyber Battle Woman: Bodjona Essosolam Trinity-Wonder

<img src="/assets/images/cyberbattle-bestwoman.jpg" alt="Best Cyber Battle Woman: Bodjona Essosolam Trinity-Wonder" style="border-radius: 10px; width: 65%;" />

**Bodjona Essosolam Trinity-Wonder** (`Tr1n1tyX_`) competed with focus and consistency throughout. The award wasn't ceremonial. She went on to win best female participant at the national ANCY competition.

---

## The origin: Edition 0 (2023)

Before there was a platform, a scoreboard, or a CTFd instance, there was just the club and a decision.

The club had just been founded. I was in my final year of undergraduate studies. We were discussing steganography, cryptography, CTF competitions, things most students had never encountered in class. We decided to organize one ourselves. We had no resources. We did it anyway.

**How we built it without CTFd:**

> We looked at CTFd and dropped it fast: too many parameters, hosting constraints, infrastructure overhead. We had to get creative.

| Component | Solution |
|-----------|---------|
| Challenge descriptions | A **Notion page** designed by me, [view it here](https://www.notion.so/IPNET-Cyber-Battle-051fca4195ab4e3b817588ebf22d0628) |
| Challenge files | A shared **Google Drive** (steganography, cryptography, forensics) |
| Submissions | Two **Telegram groups**, one per team |
| Flag validation | A custom **flag checker** |
| Scoreboard | A custom page with an **admin panel** |

I also wrote a **steganography course** specifically to prepare participants: [read it on Notion](https://calm-bite-442.notion.site/Journ-e-2-Parlons-St-ganographie-85dbeebfec254cda950a8be7c6eeadc2).

**Categories:** Steganography · Cryptography · Forensics

Zero budget. No sponsorship. No external support. Just students who wanted to build something that didn't exist yet at their school.

That edition proved the concept. Two years later: CTFd, 13 teams, 7 categories, a national audience, and three official awards.

---

**Organized by ASSIMTI Essowèréou Charles**: President, IPNET CyberSec Club · Co-signed: PAWOU P. BATANA
