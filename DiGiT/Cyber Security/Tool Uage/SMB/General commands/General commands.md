view shares on a box:
```
smbclient -L <targetIP> -N
```

Try to connect to shares:
```
smbclient '\\<targetIP>\<ShareName>'
```

Connect using a username:
```
smbclient '\\<targetIP>\<shareName> -U <username>'
```