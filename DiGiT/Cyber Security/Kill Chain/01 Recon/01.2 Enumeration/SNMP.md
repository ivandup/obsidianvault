# onesixone
Can be used to brute force SNMP community names
```
onesixone -c /usr/share/doc/onesixone/dict.txt <targetIP>
```

Example Output:
![[Pasted image 20260421065733.png]]

Check if a specific community string exists:
snmpwalk -c public targetIP snmpVersion

```
snmpwalk -c public 192.168.1.10 v1
```

Get the private string:
```
snmpwalk -c private 10.211.55.4 -v1 -On |grep '1.3.6.1.2.1.1.5'
```

Example output:
![[Pasted image 20260421070150.png]]

Change the SNMP string to HACKED
```
snmpwalk -c private 10.211.55.4 .1.3.6.1.2.1.1.5.0 s HACKED
```

Example outout:
![[Pasted image 20260421070444.png]]

# snmpenum
```
snmpenum -t 192.168.1.X
```

# snmpcheck

```
snmpcheck -t 192.168.1.X -c public
```