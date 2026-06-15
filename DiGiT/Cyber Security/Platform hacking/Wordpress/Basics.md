When you discover a progress site, run the basics first.
Start with getting a directory listing:
```
gobugobuster dir -u http://10.50.10.70 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
```

Search for specific files:
```
sudo ffuf -c -ic -u http://10.50.10.70/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -e .php,.txt,.html
```

## Run wpscan
Refer to Tool Usage > wpscan for more details

