# Printer Shares — picoCTF Writeup

## Description
Someone accidentally sent an important file to a network printer. Retrieve it from the print server running on `mysterious-sea.picoctf.net:62443`.

---

## Solution

### Step 1 — Verify the port is open
```bash
nc -vz mysterious-sea.picoctf.net 62443
```
Output confirmed the port is open.

### Step 2 — Identify the service
```bash
nmap -sV -p 62443 mysterious-sea.picoctf.net
```
```
PORT      STATE SERVICE     VERSION
62443/tcp open  netbios-ssn Samba smbd 4
```
The service is **SMB (Samba)** — a file sharing protocol.

### Step 3 — List available shares
```bash
smbclient -L //mysterious-sea.picoctf.net -N -p 62443
```
```
Sharename   Type    Comment
---------   ----    -------
shares      Disk    Public Share With Guests
IPC$        IPC     IPC Service (Samba 4.19.5-Ubuntu)
```
Found a public share called `shares` with guest access — no password needed.

### Step 4 — Connect and browse the share
```bash
smbclient //mysterious-sea.picoctf.net/shares -N -p 62443
smb: \> ls
```
```
dummy.txt    N    1142    Wed Feb  4 16:22:17 2026
flag.txt     N      37   Fri Mar  6 15:25:41 2026
```

### Step 5 — Download and read the flag
```bash
smb: \> get flag.txt
$ cat flag.txt
```

---

## Flag
```
picoCTF{5mb_pr1nter_5h4re5_2f61915b}
```

---

## Key Concepts
| Concept | Detail |
|---|---|
| SMB Protocol | Server Message Block — Windows/Linux file sharing |
| `nmap -sV` | Service version detection |
| `smbclient -L` | List shares on an SMB server |
| `-N` flag | No password / anonymous/guest access |
| `netbios-ssn` | Indicator of SMB service |
