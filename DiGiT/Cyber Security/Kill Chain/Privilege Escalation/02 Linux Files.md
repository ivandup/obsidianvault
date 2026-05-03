Find readable config files
```
find /etc/ -readable -type f 2>/dev/null
find /etc/ -readable -maxdepth 1 -type f 2>/dev/null
```


```
ls -aRl /etc/ |awk '$1 ~ /^.*w*/' 2>/dev/null # Find files with read permissions
ls -aRl /etc/ |awk '$1 ~ /^..w/' 2>/dev/null # For a specific owner:
```

# Check the log files
Check for interesting info in the logs files like Apache, MySQL, mail, etc.

# Jail Breakout
Some commands to try:
```
python -c 'import pty;pty.spawn("/bin/bash")'
echo os.system('/bin/bash')
/bin/sh -i
```

# File system mounts:
```
mount
df -h
cat /etc/fstab
```

# What advanced Linux system file permissions are used
Example, what files are executed by root but can be modified by anyone
```
find / -perm -1000 -type d 2>/dev/null # User
find / -perm -g=s -type f 2>/dev/null # group
```

Look in common place for files with special permissions:
```
for i in `locate -r "bin$"`; do find $i \( -perm -4000 -o -perm -2000\) -type f 2 2>/dev/null; done
```

Where can I write or execute from?
```
find / -writable -type d 2>/dev/null
```
