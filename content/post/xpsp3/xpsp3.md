---
title: Windows XP SP3
date: 2023-07-18
description: Windows XP SP3 x86
tags: 
    - nmap
categories:
    - Windows XP SP3 x86
    - Parrot OS
draft: true
---

```shell
┌─[parrot@parrot]─[~]
└──╼ $nmap -p 135 --script vuln 10.0.2.33 -Pn
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-26 13:27 GMT
Nmap scan report for 10.0.2.33
Host is up (0.031s latency).

PORT    STATE SERVICE
135/tcp open  msrpc

Nmap done: 1 IP address (1 host up) scanned in 27.31 seconds
```

```shell
┌─[parrot@parrot]─[~]
└──╼ $nmap -p 445 --script vuln 10.0.2.33 -Pn
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-26 13:31 GMT
Nmap scan report for 10.0.2.33
Host is up (0.011s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED

Nmap done: 1 IP address (1 host up) scanned in 27.23 seconds
```


```shell
┌─[parrot@parrot]─[~]
└──╼ $nmap -p 139 --script vuln 10.0.2.33 -Pn
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-26 13:37 GMT
Nmap scan report for 10.0.2.33
Host is up (0.027s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn

Host script results:
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_smb-vuln-ms10-054: false

Nmap done: 1 IP address (1 host up) scanned in 18.24 seconds
┌─[parrot@parrot]─[~]
└──╼ $
```

