## Method 1: pemcracker

Check the file type

```
file id_rsa
```

Is should say PEM RSA private key

To crack is, run:
```
pemcracker id_rsa rockyou.txt
```

## Method 2: John the ripper

Convert the SSH key to a format john can understand:
```
ssh2john <KeyFileYouCopied> > sshkey
```

Run the cracker:
```
john sshkey --wordlist=/usr/share/wordlists/rockyou.txt
```

