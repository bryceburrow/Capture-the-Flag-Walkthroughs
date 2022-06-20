# Task 1 - Simple CTF

In for this machine the first scans I started were nmap, gobuster and enum4linux.

Enum4linux didn't return anything useful.

Going to the machines website we see the default Apache server page. 

Once our nmap scan is finished we can see there are 2 services that are operating under port 1000, port 21 and port 80.

### How many services are running under port 1000?
2

### What is running on the higher port?

When running the scan on all ports we can see there is another port open on 2222 that is OpenSSH

Answer: ssh

### What's the CVE you're usng against the application?

From here we don't have too much information, from the gobuster scan we started earlier we can see there is a directory called /simple

Going to this page we can see there is a service called CMS Made Simple version 2.2.8 running on it.

Let's do some reseach to see if there is a CVE that is associated with this version.

When searching for CMS Made Simple 2.2.8 the first page is from Exploit DB, the CVE for this exploit is: CVE-2019-9053

### What kind of vulnerability is the application vulnerable?

We can see from that same ExploitDB page that the vulnerablility is a SQL injection. The abbriviation for this would be: *sqli*

### What's the password?

Using the exploit from ExploitDB we can run that and set it to crack the password using the following command.

python 46635.py -u http://10.10.203.55/secret --crack -w /usr/share/wordlists/rockyou.txt

once the script goes through all of the possible passwords we get the answer: *secret*

Username: mitch

### Where can you login with the details obtained?

For this answer I had run another gobuster scan on the /simple directory which showed me the /admin page > I was able to log in with the credentials we got but that does not answer the question.

SSH was running on the higher port we found earlier so I tried logging in there and that was successful.

ssh mitch@10.10.203.55 -p 2222

Answer: SSH

### What is the user flag?

we are able to run ls and find user.txt > getting the flag G00d j0b, keep up! after running cat user.txt

### Is there any other user in the home directory? What's its name?

we can see the contents of the home directory by running ls /home
Answer: sunbath

### What can you leverage to spawn a privileged shell?

Running the command sudo -l we can see what sudo commands can be run without a password and we get the following output

User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim

The answer would be vim

### What is the root flag?

I did some research on how to spawn a privileged shell using vim. From GTFOBins, which is a great resource for spawning escalated shells through various tools I found the following command

sudo vim -c ':!/bin/sh'

When we run this command with sudo we are able to run id after and we can see we are now root

uid=0(root) gid=0(root) groups=0(root)

Now we can move into the root directory with cd /root and in that directory we have root.txt > we can find the contents of this file with cat root.txt

Answer: W3ll d0n3. You made it!

This was another easy machine we were able to search for the vulnerability for the system and then use the shell we have to escalate our privileges through vim.