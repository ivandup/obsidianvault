Check out robots.txt for anything interisting
```
http://10.50.10.139/robots.txt
```

Run GoBuster to get directory listing
```
gobuster dir -u http://10.50.10.139 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.html,.txt,.sh,.js,.bak
```

Run nikto to cheack for any vulnerabilities
```
nikto -h http://10.50.10.139
```

Check on the website for any possible SQL injection
Example URL:
```
http://10.50.10.139/?nid=1
```

Run SQLmap:
```
sqlmap -u 'http://10.50.10.139/?nid=1'
```