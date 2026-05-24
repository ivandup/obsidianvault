# Cracking SSH Private Keys (id_rsa)

Convert the SSH key to a format john can understand:
```
ssh2john <KeyFileYouCopied> > sshkey
```

Run the cracker:
```
john sshkey --wordlist=/usr/share/wordlists/rockyou.txt
```

