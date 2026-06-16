# Robots.txt
Check out robots.txt for anything interesting
```
http://10.50.10.139/robots.txt
```

# Directory Listing
Run GoBuster to get directory listing
```
gobuster dir -u http://10.50.10.139 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.html,.txt,.sh,.js,.bak
```

# Vulnerability Scan
Run nikto to cheack for any vulnerabilities
```
nikto -h http://10.50.10.139
```

# Check SQL Injection
Check on the website for any possible SQL injection
Check out Tool Usage > SQLmap

