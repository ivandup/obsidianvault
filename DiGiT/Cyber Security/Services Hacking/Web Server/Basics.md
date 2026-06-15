If a web port is open, run the following checks:
# Directory Paths
Enumerate directory paths
## GoBuster:
```
gobuster dir -u http://192.168.88.129 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html -t 40 -e

# OR

gobuster dir -u http://192.168.88.129 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

## fuff:
```
ffuf -w /path/to/wordlist -u http://targetHost.com -e .php,.txt -of html -o default)first_scan.html -c -fs 462
```
-e => Files you are looking for
-c => For colour
-o => Output
# Vulnerability Scan
Run a vulnerability scan using Nikto:
```
nikto -h http://10.50.10.67
```