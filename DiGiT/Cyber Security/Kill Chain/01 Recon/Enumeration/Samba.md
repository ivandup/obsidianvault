
Enumerate Samba
### smbmap
```
smbmap -H <ipAddress>
```

![[Pasted image 20260421064622.png]]

### smbclient
```
smbclient -L <targetIP>
```

```
rpcclient -U "" -N <targetIP>
```

# enum4linux
```
enum4linux -a <targetIP>
```

# nmap
```
nmap -sS -T4 --script vuln <targetIP>
```