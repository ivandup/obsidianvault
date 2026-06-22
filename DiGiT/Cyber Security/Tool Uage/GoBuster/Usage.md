# GoBuster
Use GoBuster to see if you can find any directory listings:
```
gobuster dir -u http://10.50.10.68 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

OR 

```
gobuster dir -u http://10.50.10.68 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.html,.txt,.sh,.js,.bak
