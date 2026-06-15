If a device is running Samba, do some enumeration:
```
enum4linux 10.50.10.67
```

You will get possibly get some shares and username.

# Check shares listed:
```
smbclien -L 10.50.10.67
```

# Connect to a Share
To connect to a share and see what is listed there, use the following command:
```
smbclient //10.50.10.67/ITDEPT
```

Check is anything usefull in the shares:
```
ls
```

# Upload
Check if you can upload anything to the share
Try uploading any random file
```
put <fileName.txt>
```
