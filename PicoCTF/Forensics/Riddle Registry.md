# Riddle Registry – picoCTF Write-up


## Challenge Description

![Challenge page](./assets/1__challenge.png)

We are given a PDF file described as a "Hidden Confidential Document." The hint suggests that the flag is hidden within the **metadata** of the file rather than its visible content.

---

## Step 1 — Opening the PDF

Opening the PDF shows a document full of random text and redacted sections designed to mislead you. The flag is **not** in the visible content.

![PDF contents](./assets/2__pdf.png)

---

## Step 2 — Extracting Metadata

Since the hint points to metadata, we use `exiftool` to inspect the file:
```bash
exiftool confidential.pdf
```

### Output
```
Author : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jMjA3MzY2OX0=
```

![Exiftool output showing suspicious Author field](./assets/3__exiftool.png)

The **Author** field contains a suspicious string ending in `=` — a strong indicator of **Base64 encoding**.

---

## Step 3 — Decoding the Base64 String

I used **CyberChef** to decode the string:

1. Paste the encoded string into the Input box
2. Apply the **"From Base64"** operation

### Decoded Result
```
picoCTF{puzzl3d_m3tadata_f0und!_c2073669}
```

![CyberChef decoding the Base64 string to reveal the flag](./assets/4__cyberchef.png)

---

## Flag
```
picoCTF{puzzl3d_m3tadata_f0und!_c2073669}
```



## What I Learned

- Metadata is a common hiding place in forensic challenges
- Tools like `exiftool` are essential for file analysis
- Base64 encoding is frequently used in CTFs
- Online tools like CyberChef make decoding quick and easy



