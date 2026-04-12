---
title: "IPNET Cyber Battle — Togo's First University-Organized CTF"
date: 2025-07-19 00:00:00 +0000
categories: [Achievements, Events]
tags: [ctf, cybersecurity, togo, ipnet, organization, first]
image:
  path: /assets/images/cyberbattle-group.jpg
  alt: IPNET Cyber Battle 2025 participants
---

> *"Where Cyber Warriors Rise" — A milestone event in Togolese cybersecurity history, organized entirely by students.*

## Watch the Event

[![IPNET Cyber Battle — Official Video](https://img.youtube.com/vi/HdqJ9TKKMVw/maxresdefault.jpg)](https://www.youtube.com/watch?v=HdqJ9TKKMVw&t=1s)

---

## Overview

The **IPNET Cyber Battle** is the annual Capture The Flag (CTF) competition organized by the IPNET CyberSec Club at IPNET Institute of Technology, Togo.

It holds a historic distinction: **it is the first CTF competition ever organized by a university institution in Togo.** Before this event, cybersecurity competitions in the country were either informal or driven by external organizations. The IPNET CyberSec Club changed that — designing, deploying, and running a fully professional-grade CTF with a multi-stage format, custom challenges, and a competitive scoring system.

---

## The Team

![Group photo — IPNET Cyber Battle participants](/assets/images/cyberbattle-group.jpg)

---

## Inside the Competition

![Charles supervising a challenger](/assets/images/cyberbattle-supervision.jpg)

*ASSIMTI Essowèréou Charles — President of the club — actively present during the competition, supervising challengers in real time.*

![Two participants collaborating](/assets/images/cyberbattle-girls-collab.jpg)

---

## Competition Format

### Phase 1 — Pre-Qualification (OSINT + Forensics + Steganography)

Before reaching the main CTF, candidates had to pass a **multi-stage pre-qualification challenge** — a custom-designed puzzle chain created specifically to test skills across OSINT, forensics, cryptography, and steganography.

| Step | Action | Skill Tested |
|------|--------|-------------|
| 1 | Analyze a LinkedIn video containing **7 hidden QR codes** | OSINT · Visual analysis |
| 2 | Decode a **Python ASCII script** from a QR code → hidden message | Scripting · Encoding |
| 3 | Find a **QR code hidden at the end of a YouTube video** | OSINT · Attention to detail |
| 4 | Retrieve a PDF from Proton Drive — containing a **hidden text message** revealed via Word | Steganography · Document forensics |
| 5 | Search LinkedIn "À propos" for a **reversed encoded string** → decode with CyberChef | OSINT · CyberChef |
| 6 | Download `message.jpg` — fix **corrupted magic bytes** to reveal the image | File forensics · Hex editing |
| 7 | Run `steghide extract` on the corrected image → extract `secret.txt` | Steganography |
| 8 | Decode **multiple layers of Base64 + Reverse** in `secret.txt` → get ZIP password | Cryptography |
| 9 | Open `Graal.zip` → find **6 unique images** (duplicates filtered with a bash script) | Scripting |
| 10 | Assemble characters from each image → construct the final flag | Image forensics |

**Final flag:**
```
IPNET{y0u_4r3_th3_b3st!bRuh_3ssow7r3oU_g1v3_y0u_4cc3s_t0_th3_r0und_t4bl3_0f_fin4l1sT!!_65547926$$64}
```

> `3ssow7r3oU` embedded in the flag is a direct reference to the creator — ASSIMTI **Essowèréou** Charles. A signature hidden in plain sight.

---

### Phase 2 — Main CTF (Jeopardy Format)

| Aspect | Details |
|--------|---------|
| Format | Jeopardy CTF |
| Categories | Web, Cryptography, Forensics, OSINT, Reverse Engineering, Steganography, Pwn |
| Platform | CTFd |
| Organization | Entirely student-run |

---

## Scoreboard

![Scoreboard IPNET Cyber Battle 2025](/assets/images/cyberbattle-scoreboard.png)

| Rank | Team | Score |
|------|------|-------|
| 🥇 1 | default | 4 050 pts |
| 🥈 2 | g3n3s6 | 3 100 pts |
| 🥉 3 | SxY | 2 700 pts |
| 4 | Abd61 | 2 100 pts |
| 5 | t0g37h3R | 2 000 pts |
| 6 | Indice20 | 1 600 pts |
| 7 | F_$oCieTy | 1 200 pts |
| 8 | Ami1r | 1 200 pts |
| 9 | Dmod | 1 050 pts |
| 10 | F34r_DrK | 1 050 pts |

---

## Awards

### Best Cyber Battle Woman — BODJONA Essodiam T.N.

![Best Cyber Battle Woman award](/assets/images/cyberbattle-award-woman.jpg)

This award was not added as a formality. The winner went on to win **best female participant at the national ANCY competition** — confirming that given the right environment, **talent has no gender**.

| Prize | Description |
|-------|-------------|
| Top 3 Teams | Overall performance ranking |
| First Blood | Fastest solve on the hardest challenge |
| Best Writeup | Highest quality technical documentation |
| Best Cyber Battle Woman | Excellence award for the top female participant |

---

**Organized by ASSIMTI Essowèréou Charles** — President, IPNET CyberSec Club · Co-signed: PAWOU P. BATANA
