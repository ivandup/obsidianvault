Start with a nmap scan:
```
nmap -A -sV -sC --T3 -oN nmap_full_scan -p- 10.2.10.0/24
```

If you know they are only Windows Devices, you can limit the ports:
```
nmap -sS -p 80,88,135,139,389,443,445,464,636,3268,3389,5985,5986 --open -T3 0N nmap.scan 10.2.10.0/24
```

Enumerate some more by grabbing the O/S Details, hostnames & SMB signing enabled:
```
nxc smb 10.2.10.10
```
