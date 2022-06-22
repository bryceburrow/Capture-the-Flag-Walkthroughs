# Nmap 7.92 scan initiated Wed Jun 22 00:53:59 2022 as: nmap -sV -sC -T5 -p- -oN /home/bryce/Github/IgniteCTF/nmapscan.md 10.10.225.183
Nmap scan report for 10.10.225.183
Host is up (0.19s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-title: Welcome to FUEL CMS
Service Info: Host: 127.0.1.1

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jun 22 01:00:00 2022 -- 1 IP address (1 host up) scanned in 360.83 seconds
