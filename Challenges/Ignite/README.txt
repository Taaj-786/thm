##  Ignite

First of all Nmap Scan

# Nmap 7.95 scan initiated Mon Jul 21 02:21:05 2025 as: /usr/lib/nmap/nmap --privileged -sC -sV -oN nScan -T5 10.10.42.167
Warning: 10.10.42.167 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.42.167
Host is up (0.22s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Welcome to FUEL CMS
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/fuel/

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jul 21 02:21:24 2025 -- 1 IP address (1 host up) scanned in 18.53 seconds


Only one port is open so lets go 

First of all cheking if this fuel CMS has vulnerability in searchsploit..
And yes it had....

Used Remote Code Execution to get into the server.....

Uploaded a reverse_shell.php and ran it through the browser....  /shell.php

and now we have a shell.....

upgraded the shell to a terminal via

python3 -c "import pty; pty.spawn('/bin/bash')"

Uploaded linpeas and ran it.....

found a flag in home directory of a user named user.txt  

In the result of linpease i found the password  .. "mememe"

Using the password i got admin prev and root.txt was in the /root dir


# Task 1 Root it!

1. User.txt

'''
6470e394cbf6dab6a91682cc8585059b
'''

2. Root.txt

'''
b9bbcb33e11b80be759c4e844862482d
'''