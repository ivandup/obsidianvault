# Enumerate Users
```bash
nxc smb <dc_ip> --users
nxc smb <dc_ip> --rid-brute 10000 # bruteforcing RID
net rpc group members 'Domain Users' -W '<domain> -l <ip> -U '%'
```
