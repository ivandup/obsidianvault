# Enumerate Users
```bash
nxc smb <dc_ip> --users
nxc smb <dc_ip> --rid-brute 10000 # bruteforcing RID
net rpc group members 'Domain Users' -W '<domain> -l <ip> -U '%'
```

## Using NetExec
check for any devices that has SMB signing enabled: (you can use nxc or Netexec)
```shell
netexec smb 10.2.10.10-23
```

Example Outout:
```shell
netexec smb 10.2.10.10-23           
SMB         10.2.10.11      445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.2.10.10      445    KINGSLANDING     [*] Windows 10 / Server 2019 Build 17763 x64 (name:KINGSLANDING) (domain:sevenkingdoms.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.2.10.12      445    MEEREEN          [*] Windows Server 2016 Standard Evaluation 14393 x64 (name:MEEREEN) (domain:essos.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         10.2.10.22      445    CASTELBLACK      [*] Windows 10 / Server 2019 Build 17763 x64 (name:CASTELBLACK) (domain:north.sevenkingdoms.local) (signing:False) (SMBv1:None)
SMB         10.2.10.23      445    BRAAVOS          [*] Windows 10 / Server 2019 Build 17763 x64 (name:BRAAVOS) (domain:essos.local) (signing:False) (SMBv1:None)

```

# Identify any users
```shell
netexec smb 10.2.10.11 --users
```

Example Output
```shell
netexec smb 10.2.10.11 --users
SMB         10.2.10.11      445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.2.10.11      445    WINTERFELL       -Username-                    -Last PW Set-       -BadPW- -Description-                                  
SMB         10.2.10.11      445    WINTERFELL       Guest                         <never>             0       Built-in account for guest access to the computer/domain
SMB         10.2.10.11      445    WINTERFELL       arya.stark                    2026-04-05 19:30:35 0       Arya Stark 
SMB         10.2.10.11      445    WINTERFELL       sansa.stark                   2026-04-05 19:31:06 0       Sansa Stark 
SMB         10.2.10.11      445    WINTERFELL       brandon.stark                 2026-04-05 19:31:13 0       Brandon Stark 
SMB         10.2.10.11      445    WINTERFELL       rickon.stark                  2026-04-05 19:31:21 0       Rickon Stark 
SMB         10.2.10.11      445    WINTERFELL       hodor                         2026-04-05 19:31:27 0       Brainless Giant 
SMB         10.2.10.11      445    WINTERFELL       jon.snow                      2026-04-05 19:31:34 0       Jon Snow 
SMB         10.2.10.11      445    WINTERFELL       samwell.tarly                 2026-04-05 19:31:42 0       Samwell Tarly (Password : Heartsbane) 
SMB         10.2.10.11      445    WINTERFELL       jeor.mormont                  2026-04-05 19:31:50 0       Jeor Mormont 
SMB         10.2.10.11      445    WINTERFELL       sql_svc                       2026-04-05 19:31:57 0       sql service 
SMB         10.2.10.11      445    WINTERFELL       [*] Enumerated 10 local users: NORTH
```

# Creds sparying with winrm

If you have credentials, and you want to see on which other devices on the LAN they work with , you can run:
```shell
nxc winrm 10.2.10.11-23 -u jon.snow -p iknownothing 
```
# Check for the password policy:
```shell
nxc smb 10.2.10.11 --pass-pol
```

Example output:
```shell
nxc smb 10.2.10.11 --pass-pol
SMB         10.2.10.11      445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.2.10.11      445    WINTERFELL       [+] Dumping password info for domain: NORTH
SMB         10.2.10.11      445    WINTERFELL       Minimum password length: 5
SMB         10.2.10.11      445    WINTERFELL       Password history length: 24
SMB         10.2.10.11      445    WINTERFELL       Maximum password age: 311 days 2 minutes 
SMB         10.2.10.11      445    WINTERFELL       
SMB         10.2.10.11      445    WINTERFELL       Password Complexity Flags: 000000
SMB         10.2.10.11      445    WINTERFELL           Domain Refuse Password Change: 0
SMB         10.2.10.11      445    WINTERFELL           Domain Password Store Cleartext: 0
SMB         10.2.10.11      445    WINTERFELL           Domain Password Lockout Admins: 0
SMB         10.2.10.11      445    WINTERFELL           Domain Password No Clear Change: 0
SMB         10.2.10.11      445    WINTERFELL           Domain Password No Anon Change: 0
SMB         10.2.10.11      445    WINTERFELL           Domain Password Complex: 0
SMB         10.2.10.11      445    WINTERFELL       
SMB         10.2.10.11      445    WINTERFELL       Minimum password age: 1 day 4 minutes 
SMB         10.2.10.11      445    WINTERFELL       Reset Account Lockout Counter: 5 minutes 
SMB         10.2.10.11      445    WINTERFELL       Locked Account Duration: 5 minutes 
SMB         10.2.10.11      445    WINTERFELL       Account Lockout Threshold: 5
SMB         10.2.10.11      445    WINTERFELL       Forced Log off Time: Not Set

```

# nxc LDAP
Have a look if there are any quota's on machine creation
```
nxc ldap 10.2.10.11 -u jon.snow -p iknownothing -d north.sevenkindoms.local -M maq
```

Example Outout:
```shell
xc ldap 10.2.10.11 -u jon.snow -p iknownothing -d north.sevenkingdoms.local -M maq
LDAP        10.2.10.11      389    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:None) (channel binding:Never)
LDAP        10.2.10.11      389    WINTERFELL       [+] north.sevenkingdoms.local\jon.snow:iknownothing 
MAQ         10.2.10.11      389    WINTERFELL       [*] Getting the MachineAccountQuota
MAQ         10.2.10.11      389    WINTERFELL       MachineAccountQuota: 10

```
