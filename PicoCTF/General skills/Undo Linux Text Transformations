# Undo Linux Text Transformations – picoCTF Write-up

## Challenge Overview

This challenge provided a **remote server** accessible via Netcat where the flag had been scrambled using a **series of Linux text transformations**.

The objective was to **connect to the server**, read each step's hint, and **reverse the transformation using the correct Linux command** to recover the original flag.

<div align="center">

![Category](https://img.shields.io/badge/Category-General%20Skills-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Points](https://img.shields.io/badge/Points-100-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Solved%20✓-success?style=for-the-badge)

</div>

![Challenge Overview](./assets/1.png)

---

## Step 1 — Connecting & Reversing Base64

I launched the challenge instance and connected to the remote server using Netcat:

```bash
nc foggy-cliff.picoctf.net 53096
```

The server welcomed us to the **Text Transformations Challenge** and presented Step 1:

```
Current flag: KW9zOHIwMG5uLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj
Hint: Base64 encoded the string.
```

To reverse Base64 encoding:

```bash
base64 -d
```

**Output:** `Correct!`

![Connecting to server and Step 1](./assets/2.png)

---

## Step 2 — Reversing Text Reversal

```
Current flag: )os8r00nn-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc
Hint: Reversed the text.
```

To reverse a reversed string:

```bash
rev
```

**Output:** `Correct!`

---

## Step 3 — Reversing Underscore/Dash Substitution

```
Current flag: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-nn00r8so)
Hint: Replaced underscores with dashes.
```

To swap dashes back to underscores:

```bash
tr '-' '_'
```

**Output:** `Correct!`

---

## Step 4 — Reversing Bracket Substitution

```
Current flag: cvpbPGS(Eriref1at_g3kg_Ge4afs0ez@g10af_nn00r8so)
Hint: Replaced curly braces with parentheses.
```

> ⚠️ Note: My first attempt with `tr '{}' '()'` failed. The correct syntax reverses the mapping:

```bash
tr '()' '{}'
```

**Output:** `Correct!`

![Steps 2–4 in terminal](./assets/3.png)

---

## Step 5 — Reversing ROT13

```
Current flag: cvpbPGS{Eriref1at_g3kg_Ge4afs0ez@g10af_nn00r8so}
Hint: Applied ROT13 to letters.
```

ROT13 is self-inverse — applying it once reverses it:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Output:** `Correct!`

```
Congratulations! You've recovered the original flag:
>>> picoCTF{...}
```

![Step 5 ROT13 and flag revealed](./assets/4.png)

---

## Summary of Transformations

| Step | Hint | Reverse Command |
|------|------|-----------------|
| 1 | Base64 encoded the string | `base64 -d` |
| 2 | Reversed the text | `rev` |
| 3 | Replaced underscores with dashes | `tr '-' '_'` |
| 4 | Replaced curly braces with parentheses | `tr '()' '{}'` |
| 5 | Applied ROT13 to letters | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |

---


## Key Takeaways

- `nc` (Netcat) is the standard tool for interacting with CTF remote servers
- `base64 -d` decodes Base64-encoded strings on Linux
- `rev` reverses text line by line
- `tr '-' '_'` swaps dashes back to underscores
- `tr '()' '{}'` restores curly braces — argument order matters!
- `tr 'A-Za-z' 'N-ZA-Mn-za-m'` is the standard ROT13 one-liner

---

> 📁 Part of my CTF write-ups collection → [github.com/yesshhaa/CTF-](https://github.com/yesshhaa/CTF-/blob/main/PicoCTF)
