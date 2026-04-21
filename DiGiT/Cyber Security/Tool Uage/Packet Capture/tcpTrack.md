You can use tcptrack to measure the bandwidth you are using:

Installing tcptrack:
```
apt-get isntall tcptrack -y
```

-i => Interface to use
-f => follow
-r 5 => Reset after 5 seconds
port  => Port to listen on

Usage:
```
tcptrack -i eth0 -f -r 5 port 25
```

