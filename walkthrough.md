# Task 1 - Web App Testing and Privilege Escaltion

After starting the machine up by clicking "Start Machine" the first thing I think to do is run an nmap scan.

## Enumeration
The scan I used was sudo nmap -sV -sC -T5 -p- $ip -oN ~/Github/BasicPentestingCTF/nmapscan.md

-sV - searches for the versions of the services running
-sC - scans the machine using the default scripts
-T5 - the fastest nmap scan available (it's just a CTF so I'm not worried about being caught)
$ip - I had earlier set the variable ip to the ip of our machine 10.10.51.32
-oN - exports the results to the following file in the nmap format

I then ran a gobuster scan since port 80 was open.

sudo gobuster dir -u http://10.10.51.32 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 60

dir - sets the scan type to scan for directories
-u - the url to scan for
-w - the wordlist that is being used to scan the directories
-t - number of threads to run on (default 10)

in our scan we have found a directory called /development

2022/06/18 19:27:11 Starting gobuster in directory enumeration mode

/development          (Status: 301) [Size: 316] [--> http://10.10.51.32/development/]
Progress: 756 / 220561 (0.34%)                                                  Progress: 930 / 220561 (0.42%)    

What is the name of the hidden directory on the web server(enter name without /)?

development

 to get some more information I ran enum4linux -a 10.10.51.32

 from this scan we got the users

 S-1-22-1-1000 Unix User\kay (Local User)
 S-1-22-1-1001 Unix User\jan (Local User)

now that we have these users we can fill out the next question

What is the username?
jan
## Password Cracking
now that we have a username we can try brute forcing for a password

port 22 is open so I will try brute forcing ssh using hydrda
hydra -t 4 -l jan -P /usr/share/wordlists/rockyou.txt ssh://10.10.51.32
-l - what username is to be used
-P - the file with a list of passwords
ssh is the protocol we are brute forcing

What is the password?

from the hydra command we get the password to be armando

What service do you use to access the server(answer in abbreviation in all caps)?

the service or protocol was SSH

when going into the kay's directory there is another file called pass.bak

we are not able to cat out the file because of our permission level

after running ls -la for the full details of the files in kay's directory
## Final Flag
there is a hidden file named .viminfo

using this info we may be able to open the pass.bak file using vim

vi pass.bak

this allowed the file to be opened and we can answer the final question

What is the final password you obtain?

heresareallystrongpasswordthatfollowsthepasswordpolicy$$
