Once you have shell access, check what privileges the user has:
```
whoami /priv
```

Example output:
```
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

Check if "***SeImpersonatePrivilege***" is enabled

If that is set, you can make use of GodPotato to escalate privileges.
Check out "Tool Uage > C2C Servers > Mythic > GodPotato"
