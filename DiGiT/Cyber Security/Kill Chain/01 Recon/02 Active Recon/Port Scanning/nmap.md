
![[Pasted image 20260421062527.png]]

## Cleanup the scan:
-oG => Grep all output
Only display hosts which are up and print second field.
This will only display the IP's of the devices which are up
```
nmap -n -sn <iprange> -oG - |awk '/Up$/{print $2}'
```

Scan specific Port (e.g.: port 25) using syn scan (not so stealth):
-sS => Syn Scan
```
nmap -sS -p25 <ipaddress>
```

-sA => Send ack packets. Some new NGF's will ignore the ack packets assuming that syn packet has already been sent.
# nmap parmeters:
```
# Ping Scan: 
nmap -sn <ipRange> 

# ARP Scan: 
nmap -sn -PR <ipRange> 

# UDP ping scan 
nmap -sn -PU <ipRange> 

# ICMP Echo Ping scan 
nmap -sn -PE <ipRange> 

# Mask Ping scan (use if ICMP is blocked) 
nmap -sn -PM <ipRange> 

# ICMP timestamp scan 
nmap -sn -PP <ipRange> 

# tcp syn ping scan nmap -sn -PS <ipRange> 

# IP protocol scan.use different protocols to test the connectivity 
nmap -sn -PO <ipRange>

# X-Mas scan; all TCP flags will be on, e.g.: TCP reset flag, syn, fin, urg, etc.
nmap -sX <iprange>

# Null scan, no TCP flag enabled, oppisite of x-mas scan
nmap -sN <iprange>

# Scan UDP ports
nmap -sU <ipRange>

# Scan for versions
nmap -sV <ipRange>

# Scan for vulnerabilities (e.g.: SMB):
# Scan Samba and look for Vulnerabilities sudo 
nmap -T4 -Ss -p 139,445 - -script vuln <ipRange>

# Timing (T1 - T5): (T1 => slow, T5 => fast (Agressive), T3 => default option)
nmap -sV -T1 -p25 <iprange>

# Search for domain controller:
nmap -p389 -sV <ipRange>

```

Scan entire range:
```
nmap -sP <ipRange/subnet>
```

# Host discovery Scans
```
nmap -sn -PR [IP]
# -sn: Disable port scan - 
# -PR: ARP ping scan

nmap -sn -PU [IP]
# -PU: UDP ping scan

nmap -sn -PE [IP or IP Range]
# -PE: ICMP ECHO ping scan

nmap -sn -PP [IP]
# -PP: ICMP timestamp ping scan

nmap -sn -PM [IP]
# -PM: ICMP address mask ping scan

nmap -sn -PS [IP]
# -PS: TCP SYN Ping scan

nmap -sn -PA [IP]
# -PA: TCP ACK Ping scan

nmap -sn -PO [IP]
# -PO: IP Protocol Ping scan

```

# Port and Service Discovery

```
nmap -sT -v [IP]
# -sT: TCP connect/full open scan 
# -v: Verbose output 

nmap -sS -v [IP]
# -sS: Stealth scan/TCP hall-open scan

nmap -sX -v [IP]
# -sX:** Xmax scan

nmap -sM -v [IP]
# -sM: TCP Maimon scan

nmap -sA -v [IP]
# -sA:** ACK flag probe scan

nmap -sU -v [IP]
# -sU: UDP scan

nmap -sI -v [IP]
# -sI: IDLE/IPID Header scan

nmap -sY -v [IP]
# -sY: SCTP INIT Scan

nmap -sZ -v [IP]
# -sZ:** SCTP COOKIE ECHO Scan

nmap -sV -v [IP]
# -sV: Detect service versions

nmap -A -v [IP]
# -A: Aggressive scan
```

![[Pasted image 20260421062139.png]]

## Port Specification

![[Pasted image 20260421062312.png]]
## Service and Version Detection

![[Pasted image 20260421062223.png]]


![[Pasted image 20260421062511.png]]

# Scan Phones for ADB:

To Scan a range op IP's and only display the devices which has the open ports:
```
nmap -p 5555 --open <ipRange>
```

