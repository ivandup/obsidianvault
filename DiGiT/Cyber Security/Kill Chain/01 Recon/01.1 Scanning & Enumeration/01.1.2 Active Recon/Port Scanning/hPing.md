hping using stealth scan on port 80
-S => Stealth Scan 
-p 80 => Scan port 80
```
hping3 -S <ipAddress> -p 80 -c 5
```

### TraceRoute Scan:
```
hping3 --traceroute -V -1 <targetIP>
```

Example Output:
![[Pasted image 20260421061331.png]]


```
hping3 -V -S -p 80 -s 5050 >targetIP>
```

-V => 
-S => Syn Packets
-p 80 => Port 80
-s 5050 => Source port/local port
 
 Example output:
 ![[Pasted image 20260421061513.png]]