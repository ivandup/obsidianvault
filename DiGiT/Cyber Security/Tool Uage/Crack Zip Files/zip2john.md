To get zip password hashes and use john to crack them, you can use:
```
zip2john <zipFileNme.zip > ziphash.txt
```

Crack the password:
```
john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt
```