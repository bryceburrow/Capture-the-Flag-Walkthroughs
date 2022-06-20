# Task 1 - Pickle Rick

### Enumeration

I ran an nmap scan as well as a gobuster scan for directory scanning.

The nmap scan showed that ports 22 and 80 were open meaning, http and ssh.

sudo nmap -sV -sC -T5 -p- 10.10.5.92 -oN ~/Github/PickleRickCTF/nmapscan.md

gobuster dir -u http://$ip -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 60

From the gobuster scan we get 2 results, /assets, /server-status

After going to the webiste on port 80 and viewing the source code (Ctrl+U) we find the Username: R1ckRul3s

when trying to log in to SSH using this username shows us we need a public key to gain access

After poking around on the different pages on the web server I checked for a robots.txt file

It did exsist with the following contents: Wubbalubbadubdub

I will assume this may be a password so we have

Username: R1ckRul3s
Password: Wubbalubbadubdub

the gobuster also found another page named portal.php, we may be able to use the credentials we found in on this login page

Those credentials did let us in, now we can see a command panel

Using the command "ls" gives us the following output.

Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt

From here I tried a couple of different linux commands that may allow us to move around within the command panel.

List of working commands: ls, cd, less

using the less command on the Sup3rS3cretPickl3Ingred.txt file we are able to get the first flag

### What is the first ingredient Rick needs?

mr. meeseek hair

Since we are able to use the cd command we can go to the /home directory and ls to find all of the users on the machine

We can see one of the users is Rick

we can chain commaands together using the ";" character so using the command cd /home/rick; ls we can find all of the files in the rick directory

### What is the second ingredient Rick needs?

We get one file called second ingredients which we can get the contents of using the command "cd /home/rick; less second\ ingredients"

The second flag is: 1 jerry tear

Not sure where to go from here I searched around on the internet for some guidance on manipulating a command line from within an html page

Finally I have found that if we use the command "grep -R "" " that would recursivley grep the entire directory.

Viewing the page source after grepping the entire directory we can find the commands that are disabled in the command line: ("cat", "head", "more", "tail", "nano", "vim", "vi")

sudo is not a part of the disabled commands so we can try using "sudo ls" to see if that is successful > yes we are able to use sudo commands

when trying to cd into the root directory it will not let me > cd /root

### What is the final ingredient Rick needs?

since we are able to use sudo commands we can use sudo ls ../../../../* > "../../../../" is able to move us back directories until we get to the base directory > the use of "*" allows us to ls all of the directories in the base directory.

once we find the root directory
../../../../root:
3rd.txt
snap

we can see there is a txt file named 3rd.txt using the less command we can get the contents of this file

using the command less /root/3rd.txt we are able to get the final flag: fleeb juice

That is the end of this room, a really easy room that was focused on teaching the basics of moving around the linux file system.
