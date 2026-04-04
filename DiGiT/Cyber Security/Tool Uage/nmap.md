--------------


check which ip are alive:
```
nmap -sP <ipAddressOrIPRange>
```


#### pipe to another file and only print the IP's which are UP (and print the second field)
```
nmap -sP <ipAddressOrIPRange> -oG - | awk '/Up$/{print $2}'
```

example out:
```
10.10.10.1
10.10.10.2
```


#### trace packages from your PC to remote PC (Almost like wireshark), will also show ports open when you close:
```
nmap  --packet-trace <ipAddress>
```

### To show versions of ports open
```
nmap -sV -p 21,22,80 <ipAddress>
```

#### Speed up or down scanning, example with packet flows:
```
nmap  --packet-trace -T1 <ipAddress>
```
-T1 => slow (stealth)
-T5 => Fastest scan

#### Identify target OS and additional info (It's noiser than normal options)
```
nmap -A -O <ipAddress>
```

#### TTL to identify O/S:
Linux = 64 TTL
Windows 128 TTL

#### Stealth Syn Scan (Not stealthy) will preform handshake:
```
nmap -sS <ipAddress>
```

#### UDP Scans (Can have false positives because it's does not do a handshake)
```
nmap -sU <ipAddress>
```

#### Preform NULL scan, send tcp flags with nothing enables
```
nmap -sN <ipAddress>
```

#### X-Mas Scan (All TCP flag are on)
```
nmap -sX <ipAddress>
```

#### Scan all ports
```
nmap -sV -p- <ipAddress>
```

-f command, send small IP fragments, the firewall/IDS will not see the entire packet at once
#### Can be used to try and bypass firewall restrictions
```
nmap -sV -f <ipAddress>
```
