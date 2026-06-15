Example of checking Wordpress HASHed passwords:
You a user file (wp-users.hash) that looks like this:
```
micheal:$P$BjRvZQ.VQcGZlDeiKToCQd.cPw5XCe0
steven:$P$B6X3H3ykawf2oHuPsbjQiih5iJXqad.
```

Cracking it by running:
```
john wp-users.hash --wordlist=/usr/share/wordlists/rockyou.txt 
```
