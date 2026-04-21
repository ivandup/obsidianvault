
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

![[Pasted image 20260421062021.png]]

![[Pasted image 20260421062549.png]]
# Target Specific

![[Pasted image 20260421062106.png]]