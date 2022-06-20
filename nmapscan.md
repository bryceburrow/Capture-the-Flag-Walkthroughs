# Nmap 7.92 scan initiated Mon Jun 20 01:25:26 2022 as: nmap -sV -sC -T5 -p- -oN /home/bryce/Github/PickleRickCTF/nmapscan.md 10.10.5.92
Nmap scan report for 10.10.5.92
Host is up (0.18s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:cc:f2:f6:85:87:cb:20:1e:bc:33:d5:22:ba:89:61 (RSA)
|   256 6b:60:09:c2:5a:d3:4a:2b:38:83:a7:00:54:fc:9e:fa (ECDSA)
|_  256 1e:9a:31:7b:2c:de:78:4b:dd:04:72:db:2b:cc:38:ee (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 20 01:27:51 2022 -- 1 IP address (1 host up) scanned in 145.51 seconds
