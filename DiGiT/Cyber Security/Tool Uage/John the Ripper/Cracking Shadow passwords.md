Extraction: Copying the /etc/passwd and /etc/shadow files (requires root privileges).

Combination: Merging the two files into a single cracking format using the unshadow command.

Cracking: Running the combined file against a dictionary/wordlist using John the Ripper or Hashcat.

The Unshadow Process:
```
sudo unshadow /etc/passwd /etc/shadow > /tmp/unshadowed_hashes.txt
```

Cracking with John the Ripper:
```
sudo john --wordlist=/usr/share/wordlists/rockyou.txt /tmp/unshadowed_hashes.txt
```