Enumerate users using msfconsole:
```
msfconsole
search finder
use auxiliary/scanner/finger/finger_users
set RHOSTS <targetIP>
exploit
```