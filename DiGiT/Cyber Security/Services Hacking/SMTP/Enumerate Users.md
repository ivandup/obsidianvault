# msfconsole
You can enumerate users using msfconsole
```
mfsconsole
search smtp
use auxiliary/scanner/smtp/smtp_enum
show options
set RHOSTS <targetIP>

# Verify it has been set
options 

# Run the enumeration
exploit 
```

# Enumerate using smtp-user-enum
```
smtp-user-enum -M VRFY -U /usr/share/wordlists/seclists/Usernames/Names/names.txt -t 10.50.10.77
```

The `-M` flag selects the method. Try each:

bash

```
# VRFY method (RFC command for user verification)
smtp-user-enum -M VRFY -U usernames.txt -t 10.50.10.77

# EXPN method (expand mailing lists)
smtp-user-enum -M EXPN -U usernames.txt -t 10.50.10.77

# RCPT TO method (most likely to work when VRFY is disabled)
smtp-user-enum -M RCPT -U usernames.txt -t 10.50.10.77
```

# Wordlists
If your wordlist doesn't contain any of those, you won't find anything. Use a wordlist that includes common Unix usernames:

```bash
# Common Unix usernames
/usr/share/wordlists/metasploit/unix_users.txt
/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt
/usr/share/wordlists/seclists/Usernames/Names/names.txt
```