### 1. Initial Reconnaissance
The first step in any assessment is reconnaissance to gather information as possible the target machine port scanning to discovery service and version.

```
┌──(kaushik㉿zerodayforge)-[~]
└─$ nmap -sC -sV 10.48.160.4                    
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-27 19:57 +0530
Nmap scan report for 10.48.160.4
Host is up (0.029s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e2:5a:02:db:65:e3:53:fc:3b:43:83:8d:a8:76:84:3f (RSA)
|   256 8f:fe:ea:44:bf:72:e3:97:08:f5:40:b7:b8:27:b1:75 (ECDSA)
|_  256 53:8d:3d:d0:0f:11:0a:aa:2a:c5:67:58:03:ee:06:0d (ED25519)
53/tcp open  domain  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Recruit
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.98 seconds
```


![[Screenshot_2026-09-27_20-23-47.png]]

I performed directory brute-forcing using gobuster  to find hidden path
![[Screenshot_2026-09-27_20-42-14 1.png]]
I discovery a /mail directory containing a mail.log file
![[Screenshot_2026-09-27_20-27-29 3.png]]
The API can access outside URLs. I tested it for SSRF and LFI, and managed to use it to access the server’s local `config.php` file despite the filters.
![[Screenshot_2026-09-27_20-29-55 1.png]]
I found the HR user’s username and password, logged in as HR, and got the first flag.

![[Screenshot_2026-09-27_20-32-57 1.png]]

The dashboard had a search box for finding candidates. I entered a single quote (`'`) to test for SQL Injection. The website showed a database error, which confirmed that SQL Injection was possible. Then I used the Access API to read `dashboard.php` and see how the SQL query was written, so I could understand how the vulnerability worked.

![[Screenshot_2026-09-27_20-39-04 2.png]]![[Screenshot_2026-09-27_20-40-09 1.png]]![[Screenshot_2026-09-27_20-41-16 1.png]]