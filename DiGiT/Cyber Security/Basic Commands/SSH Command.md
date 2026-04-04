#### SCP Copy from remote server

```
scp -T <user>@<server>:"c:\Windows\test.txt" <filePath>
```
-T => Translate the "\" from the windwos server

#### SSH and execute a command
```
ssh -C <user>@<server> "<commandToExecute>"
```

-C => Compress packets (Nice to hide some traffic)

#### Local Port forwarding:
```
ssh -L 2222:google.com:80 <destinationServer>
```
-L 2222:google.com => Local port 2222:<serverToConnectTo> <targetServerToTunnelVia>
