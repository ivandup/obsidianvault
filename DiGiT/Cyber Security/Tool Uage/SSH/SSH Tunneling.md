Remote machine you connecting to:
SSH and forward local port 8181 to host 10.10.10.2 and forward to port 21
```
ssh -L <localPort>:<remoteServerYouWantToConnectTo>:<redirectPort> user@server
ssh -L 8181:10.10.10.2:21
```

-L => local port

Now FTP:
ftp 127.0.0.1 8181

# Reverse port forwarding
This will tell the tunnel to connect back to your PC.

Create a remote/reverse tunnel, for example connect ot OpenVAS. 
Run this on the remote PC,
ssh -R 8181:localhost:9392 user@remotePC
```
ssh -R <localport>:<localhost:<portToListen>
```
-R => remote/reverse

Now SSH to the box

# Dynamic Tunneling

ssh -D 8181 user@server
```
ssh -D <dynamicPort> user@server
```

You can now specify a proxy server (SOCKS5), e.g. in your browser.

You can use proxy-chains for connections:

Configure proxy chains:
```
vi /etc/proxychains.conf
```

Choose proxy methos, most used is dynamic_chain
Enable 
```
proxy_dns
```

enable socks5, it's better than socks4, has more authentication methos and security features.
Specify your socks5 proxy address and port.
Once done, save and exit

Make use of proxy chains (e.g.: firefox):
```
proxychains firefox
```

You can tunnel any traffic via proxychains.
example:
```
proxychains nmap -sV -p22 10.211.55.7
```










