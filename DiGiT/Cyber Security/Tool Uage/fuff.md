Search for specific files:
```
sudo ffuf -c -ic -u http://10.50.10.70/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -e .php,.txt,.html
```