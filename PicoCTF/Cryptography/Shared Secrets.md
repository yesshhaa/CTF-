# Shared Secrets – picoCTF Write-up

## Challenge Overview

A message was encrypted using a **shared secret** — but it looks like one side of the exchange leaked something. The goal was to piece together the secret and recover the flag.

This challenge involved a **Diffie-Hellman key exchange** where Bob's private key `b` was accidentally leaked in the output file. The encryption used only a single byte of the shared secret (`shared % 256`) to XOR the flag, making it trivially decryptable once the shared secret was computed.

<div align="center">


</div>

![Challenge Overview](./assets/Screenshot_2026-05-17_231954.png)

---

## Vulnerability Analysis

Inspecting the source code revealed the encryption logic:

```python
shared = pow(A, b, p)
enc = bytes([x ^ (shared % 256) for x in flag])
```

Two critical weaknesses:

1. **Bob's private key `b` was leaked** in the output file alongside the ciphertext
2. **Only 1 byte of the shared secret** (`shared % 256`) was used as the XOR key

Since `b` was already known, we could directly compute:

```
shared   = A^b mod p
key_byte = shared % 256
flag[i]  = enc[i] XOR key_byte
```

---

## Step 1 — Downloading and Inspecting the Files

After downloading `message.txt` and `encryption.py`, I navigated to the Downloads folder:

```bash
cd Downloads
ls
# encryption.py  message.txt
```

The `message.txt` contained:

```
g = 2
p = 165379893...   (1048-bit prime)
A = 771122236...   (server public key)
b = 502087552...   (leaked client private key!)
enc = cfd6dcd0fcebf9c4dbd7e0cc8cdccd8ccbe0dddb8c87d98c8889c2
```

---

## Step 2 — Writing the Decryption Script

I created `decrypt_flag.py` using a heredoc to avoid the nano editor:

```bash
cat > decrypt_flag.py << 'EOF'
enc_hex = "cfd6dcd0fcebf9c4dbd7e0cc8cdccd8ccbe0dddb8c87d98c8889c2"
enc = bytes.fromhex(enc_hex)
shared = pow(A, b, p)
key_byte = shared % 256
flag = bytes([c ^ key_byte for c in enc])
print("Decrypted flag:", flag.decode())
EOF
```

![Writing the script in terminal](./assets/Screenshot_2026-05-17_232055.png)

---

## Step 3 — Running the Script and Recovering the Flag

```bash
python3 decrypt_flag.py
```

> Note: First attempt failed due to an accidental `~` typo (`decrypt_flag.py~`). Running with the correct filename worked immediately.

### Output

```
Decrypted  the flag
```

![Flag recovered in terminal](./assets/Screenshot_2026-05-17_232117.png)

---



> Part of my CTF write-ups collection: [github.com/yesshhaa/CTF-](https://github.com/yesshhaa/CTF-/blob/main/PicoCTF)
