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